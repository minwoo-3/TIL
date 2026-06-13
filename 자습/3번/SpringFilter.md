# Spring Filter
- 자바 클래스의 일종으로 인증 등의 작업에 활용 된다
- DispatcherServlet에 의해 다뤄지기 전, 후에 동작

## 사용 방법
- 필터 클래스 만들기 -> servlet의 Filter 인터페이스를 구현해서 만듬
```
public class FirstFilter implements Filter {}
```
#### 인터페이스 메서드
- init() : 필터가 생성될 때 수행되는 메서드

- doFilter() : Request, Response가 필터를 거칠 때 수행되는 메서드

- destory() : 필터가 소멸될 때 수행되는 메서드

### 구현
- 메서드 구현
```
@Slf4j
public class FirstFilter implements Filter {
    @Override
    public void init(FilterConfig filterConfig) throws ServletException {
        log.info("FirstFilter가 생성 됩니다.");
    }

    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
        log.info("==========First 필터 시작!==========");
        filterChain.doFilter(servletRequest, servletResponse);
        log.info("==========First 필터 종료!==========");
    }

    @Override
    public void destroy() {
        log.info("FirstFilter가 사라집니다.");
    }
}
```
- Bean 객체로 등록
- FilterRegistationBean을 이용해서 필터를 등록
```
@Configuration
public class Config{

    @Bean
    public FilterRegistrationBean firstFilterRegister() {
        FilterRegistrationBean registrationBean = new FilterRegistrationBean(new FirstFilter());
        return registrationBean;
    }
}
```