# 어노테이션(Annotation)
- 다른 프로그램에게 유용한 정보를 제공하기 위해 사용되는 것으로 주석과 같은 의미

### 역할
- 컴파일러에게 문법 에러를 체크하도록 정보를 제공
- 프로그램을 빌드할 때 코드를 자동으로 생성할 수 있도록 정보를 제공
- 런타임(시작부터 끝까지 시간)에 정보 제공

### 종류
- @Component : 생성한 class를 Spring Bean(스프링 컨테이너에 등록된 객체)으로 등록

- @Bean : 제어가 불가능한 외부 라이브러리 들의 것들을 Bean으로 등록

- @Controller : Spring에게 해당 Class가 Controller의 역할을 한다고 명시

- @RequestHeader : Request의 header값을 가져올 때 사용

- @RequestMapping : @RequestMapping(value=”“)에서 요청과 value가 같으면 해당 메서드를 실행

- @RequestParam : URL에 전달되는 파라미터를 메서드 인자와 매칭시켜, 파라미터를 받아서 처리

- @RequestBody : Body에 전달되는 데이터(클라이언트가 보낸 JSON 데이터를 그래도 객체나 변수로 받음)를 메서드 인자와 매칭시켜, 데이터를 받아서 처리

- @ModelAttribute : HTTP Body 내용과 HTTP 파라미터 값들을 Getter, Setter, 생성자를 통해 주입하기 위해 model 객체를 통해서 전달

- @ResponsBody : 메서드에서 리턴되는 값이 View로 출력되지 않고 HTTP Response Body에 직접 사용시에 json,xml과 같은 데이터를 return

- @AutoWired : Bean을 주입받기위해 사용 Spring 프레임워크가 Class를 보고 Type에 맞게 Bean 주입

- @GetMapping : 데이터 조회

- @PostMapping : 데이터 생성

- SpringBootTest : 스프링 전체 환경을 테스트

- @Test : 테스트용 메서드들 표시