# Spring Security

## Spring Security란?
- 인증, 권한 관리 그리고 데이터 보호 기능등의 필 수 보안 기능을 도와주는 Spring 프레임워크
- IoC/DI 패턴을 염두해서 개발하기 쉬워짐

## Spring Security 구조
- 사용자 요청이 들어옴

- Authotication Filter가 요청 가로챔

- Authotication Manger가 요청 받음

- Manger가 Provider를 조회 및 인증 요구

- Provider가 데이터를 조회하여 UserDetails에 저장

- SecurityContextHolder에 저장 되어 유저정보를 Spring Controller에서 사용 가능하게 됨

### 내부 구조
- 사용자가 자격 증명 정보 제출

- AbstarctAuthenticationProcessingFilter가 Authentication객체 생성

- Authentication객체가 AuthenticationManager에게 전달됨

- 인증 실패 시 정보가 SecurityContextHolder의 값이 지워지고 RememberMeService.joinFail()과 AuthenticationFailureHandler가 실행됨

- 인증 성공 시 SessionAuthenticationStrategy가 새로운 로그인 알림

- Authentication이 SecurityContextHolder에 저장됨
 
- SecurityContextPersistenceFilter가 SecurityContext를 HrrpSession에 저장 이로 인해 로그인 세션정보가 저장됨

### 비교
- 구조: 로그인 --> 확인--> 저장
- 내부 구조: 로그인--> 확인-->실패/성공 분리--> 세션처리