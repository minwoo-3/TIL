# Spring Security

## 전체 구조
- 클라이언트: Postman/앱
- JwtFilter: 토큰 검사
- SecurityConfig: 통과 거절 여부 결정
- Controller: 요청 처리
- Service/DB: 저장/조회

## application.yml
```
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/spring_security_db   # MySQL 주소
    driver-class-name: com.mysql.cj.jdbc.Driver           # MySQL 드라이버 사용
    username: root                                         # DB 아이디
    password: 1234                                         # DB 비밀번호

  jpa:
    hibernate:
      ddl-auto: update     # 앱 시작할 때 테이블 자동 생성/수정
    show-sql: true         # 실행되는 SQL을 콘솔에 출력

jwt:
  secret: "my-super-secret-key..."   # JWT 서명할 때 쓰는 비밀키 (절대 노출 금지)
  expiration: 3600000                # 토큰 유효시간 (1시간 = 3600000ms)
```
- 코드의 값 설정 파일 jwt 여기서 수정 가능

## User
```
@Entity           // 이 클래스가 DB 테이블이라는 표시
@Table(name = "users")  // 테이블 이름을 "users"로 지정
@Getter           // 모든 필드의 getter 자동 생성 (Lombok)
@Builder          // User.builder().email("...").build() 방식으로 객체 생성 가능
@NoArgsConstructor  // 빈 생성자 자동 생성
@AllArgsConstructor // 모든 필드 받는 생성자 자동 생성
public class User {

    @Id  // 기본키(PK)
    @GeneratedValue(strategy = GenerationType.IDENTITY)  // 1,2,3... 자동 증가
    private Long id;

    @Column(unique = true, nullable = false)  // 중복 불가, null 불가
    private String email;

    @Column(nullable = false)
    private String password;  // BCrypt로 암호화된 값이 저장됨

    @Column(nullable = false)
    private String name;

    @Enumerated(EnumType.STRING)  // DB에 "ROLE_USER" 문자열로 저장
    private Role role;

    // 권한을 딱 2가지로 고정
    public enum Role {
        ROLE_USER,   // 일반 유저
        ROLE_ADMIN   // 관리자
    }
}
```
- DB의 users 테이블 한행 이클래스를 보고 JPA가 테이블을 만들어 줌

## UserRepository
```
public interface UserRepository extends JpaRepository<User, Long> {
    // JpaRepository를 상속받으면 save(), findAll(), findById() 등이 자동 생성됨

    Optional<User> findByEmail(String email);
    // → 메서드 이름만 써도 JPA가 자동으로 아래 SQL을 만들어줌
    // SELECT * FROM users WHERE email = ?

    boolean existsByEmail(String email);
    // → SELECT COUNT(*) FROM users WHERE email = ? (있으면 true)
}
```
- DB 접근 도구
- JPA가 메서드 이름을 분석에서 SQL 자동 생성

## DTO

```
// 회원가입 요청 시 클라이언트가 보내는 데이터
public record SignupRequest(String email, String password, String name) {}

// 로그인 요청 시 클라이언트가 보내는 데이터
public record LoginRequest(String email, String password) {}

// 로그인 성공 시 서버가 돌려주는 데이터
public record AuthResponse(String token, String email, String name) {}
```
- 클라이언트와 주고 받을 JSON 형태 정의

## UserService

```
@Service
@RequiredArgsConstructor  // final 필드를 생성자로 자동 주입 (Lombok)
public class UserService {

    private final UserRepository userRepository;
    private final PasswordEncoder passwordEncoder;

    public void register(SignupRequest request) {

        // 이메일 중복 체크
        if (userRepository.existsByEmail(request.email())) {
            throw new IllegalArgumentException("이미 사용 중인 이메일입니다");
            // 여기서 예외를 던지면 GlobalExceptionHandler가 잡아서 400 응답으로 바꿔줌
        }

        // User 객체 생성 (비밀번호는 BCrypt로 암호화)
        User user = User.builder()
                .email(request.email())
                .password(passwordEncoder.encode(request.password()))
                // "1234" → "$2a$10$sH1cmu..." 이런 식으로 암호화됨
                .name(request.name())
                .role(User.Role.ROLE_USER)  // 기본 권한은 USER
                .build();

        // DB 저장
        userRepository.save(user);
    }
}
```
- 중복체크, 암호화, 저장 처리 BCrypt로 해싱해서 저장

## JwtUtil

```
@Component  // Spring이 관리하는 Bean으로 등록
public class JwtUtil {

    @Value("${jwt.secret}")      // application.yml의 jwt.secret 값을 주입
    private String secret;

    @Value("${jwt.expiration}")  // application.yml의 jwt.expiration 값을 주입
    private long expiration;

    // 토큰 생성 - 로그인 성공 시 호출
    public String generateToken(String email, String role) {
        return Jwts.builder()
                .subject(email)               // 토큰의 주인공 (이메일)
                .claim("role", role)          // 추가 정보 (권한)
                .issuedAt(new Date())         // 발급 시간
                .expiration(new Date(System.currentTimeMillis() + expiration))  // 만료 시간
                .signWith(getKey())           // 비밀키로 서명 (위조 방지)
                .compact();                   // 문자열로 변환
        // 결과: "eyJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJ0ZXN0..."
    }

    // 토큰에서 이메일 꺼내기
    public String extractEmail(String token) {
        return getClaims(token).getSubject();
        // "eyJhbGci..." → "test@test.com"
    }

    // 토큰이 유효한지 확인 (서명 검증 + 만료 체크)
    public boolean isTokenValid(String token) {
        try {
            getClaims(token);  // 실패하면 예외 발생
            return true;
        } catch (JwtException | IllegalArgumentException e) {
            return false;  // 위조됐거나 만료된 토큰
        }
    }

    // 토큰 파싱 (내부 메서드)
    private Claims getClaims(String token) {
        return Jwts.parser()
                .verifyWith(getKey())   // 비밀키로 서명 검증
                .build()
                .parseSignedClaims(token)
                .getPayload();
    }

    // 비밀키를 SecretKey 객체로 변환 (내부 메서드)
    private SecretKey getKey() {
        return Keys.hmacShaKeyFor(secret.getBytes(StandardCharsets.UTF_8));
    }
}
```
- JWT의 `헤더.내용.서명` 발급

## CustomUserDetailsService

```
@Service
@RequiredArgsConstructor
public class CustomUserDetailsService implements UserDetailsService {
    // UserDetailsService: Spring Security가 정해놓은 인터페이스
    // "loadUserByUsername 메서드를 반드시 구현해라"

    private final UserRepository userRepository;

    @Override
    public UserDetails loadUserByUsername(String email) throws UsernameNotFoundException {
        // Spring Security가 로그인 시 자동으로 이 메서드를 호출함
        // 파라미터 이름이 Username이지만 우리는 email로 사용

        return userRepository.findByEmail(email)
                .map(user ->
                    // DB에서 찾은 User를 Spring Security가 이해하는 UserDetails로 변환
                    org.springframework.security.core.userdetails.User.builder()
                            .username(user.getEmail())
                            .password(user.getPassword())    // 암호화된 비밀번호
                            .authorities(user.getRole().name())  // "ROLE_USER" or "ROLE_ADMIN"
                            .build()
                )
                .orElseThrow(() -> new UsernameNotFoundException("유저를 찾을 수 없습니다: " + email));
    }
}
```
- `loadByUsername`을 호출해서 로그인 할 때 유저 정보 받음 Spring Security가 알아서 비밀번호 비교까지 처리

## JwtFilter

```
@Component
@RequiredArgsConstructor
public class JwtFilter extends OncePerRequestFilter {
    // OncePerRequestFilter: 하나의 요청에 딱 한 번만 실행되는 필터

    private final JwtUtil jwtUtil;
    private final CustomUserDetailsService userDetailsService;

    @Override
    protected void doFilterInternal(HttpServletRequest request,
                                    HttpServletResponse response,
                                    FilterChain filterChain) throws ServletException, IOException {

        //  요청 헤더에서 Authorization 값 꺼내기
        String authHeader = request.getHeader("Authorization");
        // 예: "Bearer eyJhbGciOiJIUzI1NiJ9..."

        //  토큰이 없거나 형식이 틀리면 그냥 통과 (인증 없이 허용된 URL은 여기서 통과)
        if (authHeader == null || !authHeader.startsWith("Bearer ")) {
            filterChain.doFilter(request, response);
            return;
        }

        //  "Bearer " 이후의 실제 토큰만 추출
        String token = authHeader.substring(7);

        //  토큰이 유효하면 인증 정보를 SecurityContext에 저장
        if (jwtUtil.isTokenValid(token)) {
            String email = jwtUtil.extractEmail(token);  // 토큰에서 이메일 꺼내기
            UserDetails userDetails = userDetailsService.loadUserByUsername(email);  // DB 조회

            // Spring Security에게 "이 사람은 인증된 사람이에요"라고 알려줌
            UsernamePasswordAuthenticationToken auth =
                    new UsernamePasswordAuthenticationToken(
                            userDetails, null, userDetails.getAuthorities());

            auth.setDetails(new WebAuthenticationDetailsSource().buildDetails(request));
            SecurityContextHolder.getContext().setAuthentication(auth);
            // 이걸 저장해놔야 컨트롤러에서 authentication.getName()으로 이메일 꺼낼 수 있음
        }

        //  다음 필터로 넘기기 (이게 없으면 요청이 멈춤)
        filterChain.doFilter(request, response);
    }
}
```
- 모든 HTTP 요청이 이 필터를 통과 토큰 검증 역할

## SecurityConfig

```
@Configuration   // 설정 클래스임을 표시
@EnableWebSecurity  // Spring Security 활성화
@EnableMethodSecurity  // @PreAuthorize 사용 가능하게 함
@RequiredArgsConstructor
public class SecurityConfig {

    private final JwtFilter jwtFilter;

    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
        http
            // JWT 사용하니까 CSRF 보호 비활성화 (CSRF는 세션 기반일 때 필요)
            .csrf(csrf -> csrf.disable())

            // JWT는 서버에 세션 저장 안 함 (STATELESS)
            .sessionManagement(session ->
                session.sessionCreationPolicy(SessionCreationPolicy.STATELESS))

            // URL별 접근 규칙
            .authorizeHttpRequests(auth -> auth
                .requestMatchers("/api/auth/**").permitAll()       // 로그인/회원가입: 토큰 없어도 OK
                .requestMatchers("/api/admin/**").hasRole("ADMIN") // /admin: ADMIN 권한 필요
                .anyRequest().authenticated()                      // 그 외: 로그인 필요
            )

            // JwtFilter를 기존 로그인 필터 앞에 끼워 넣기
            .addFilterBefore(jwtFilter, UsernamePasswordAuthenticationFilter.class);

        return http.build();
    }

    // 비밀번호 암호화 방식 (BCrypt)
    @Bean
    public PasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder();
        // "1234" → "$2a$10$..." 로 암호화, 복호화 불가능한 단방향 해시
    }

    // 로그인 인증을 처리하는 매니저
    @Bean
    public AuthenticationManager authenticationManager(AuthenticationConfiguration config)
            throws Exception {
        return config.getAuthenticationManager();
        // AuthController에서 authenticationManager.authenticate() 호출 시 사용됨
    }
}
```
- URL별로 어떤 접근 권한이 필요한지 결정

## AuthController

```
@RestController      // JSON 응답을 반환하는 컨트롤러
@RequestMapping("/api/auth")  // 이 컨트롤러의 URL 기본 경로
@RequiredArgsConstructor
public class AuthController {

    private final AuthenticationManager authenticationManager;
    private final JwtUtil jwtUtil;
    private final UserService userService;
    private final UserRepository userRepository;

    //  회원가입 API
    // POST /api/auth/signup
    @PostMapping("/signup")
    public ResponseEntity<String> signup(@RequestBody SignupRequest request) {
        // @RequestBody: JSON → SignupRequest 객체로 자동 변환
        userService.register(request);  // UserService에 처리 위임
        return ResponseEntity.ok("회원가입 완료");  // 200 OK
    }

    //  로그인 API → JWT 발급
    // POST /api/auth/login
    @PostMapping("/login")
    public ResponseEntity<AuthResponse> login(@RequestBody LoginRequest request) {

        //  이메일/비밀번호 검증 (틀리면 자동으로 예외 발생 → 401)
        authenticationManager.authenticate(
                new UsernamePasswordAuthenticationToken(request.email(), request.password())
        );
        // 내부적으로 CustomUserDetailsService.loadUserByUsername() 호출 →
        // DB에서 유저 조회 → BCrypt로 비밀번호 비교

        //  검증 성공 → JWT 발급
        User user = userRepository.findByEmail(request.email()).orElseThrow();
        String token = jwtUtil.generateToken(user.getEmail(), user.getRole().name());

        //  토큰 + 유저 정보 반환
        return ResponseEntity.ok(new AuthResponse(token, user.getEmail(), user.getName()));
    }
}
```
- 요청을 받아서 적절한 Service에 넘기고 결과 반환

## GlobalExceptionHandler

```
@RestControllerAdvice  // 모든 컨트롤러의 예외를 여기서 잡음
public class GlobalExceptionHandler {

    // IllegalArgumentException 발생 시 (예: 이메일 중복)
    @ExceptionHandler(IllegalArgumentException.class)
    public ResponseEntity<Map<String, String>> handleIllegalArgument(IllegalArgumentException e) {
        return ResponseEntity
                .badRequest()  // HTTP 400
                .body(Map.of("message", e.getMessage()));
        // 응답: {"message": "이미 사용 중인 이메일입니다"}
    }
}
```
- 예외가 터졌을 때 `500 Internal Server Error` 대신 JSON 에러 응답으로 바꿈