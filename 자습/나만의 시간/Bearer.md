# JWT Bearer

## Authorization(권한부여, 인가)
- HTTP에서 서버에 인증 정보를 보낼 때 사용하는 헤더이다
- 인증된 사용자가 특정 리소스에 작업을 수행할 권한이 있는지 확인하고 허가하는 보안 프로세스

## Bearer Token 인증
- 서버가 발근한 토큰을 클라이언트가 요청 헤더에 포함해 인증하는 방식
- Bearer {토큰} 형태로 Authorization 헤더에 추가

## Bearer의 의미
- 소지자라는 뜻
- 토큰을 가진 사람이 인증된 사용자라고 서버가 신뢰한다는 의미

## JWT와 Bearer Token
- REST API에서 세션 대신 자체적으로 정보를 담은 JWT를 사용하기 떄문에 Bearer Token 방식으로 전달한다