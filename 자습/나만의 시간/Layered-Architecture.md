# Layered Architecture(계층형 아키텍처)
- 소스코드의 역할과 관심사에 따라 계층으로 분리한 아키텍처
- 구성요소는 해당 계층에 관련된 기능만 수행
- 유지보수성이 높고 테스트가 쉽다

## 4-Tier Layered Arichitecture (N-tier)
- 구성되는 계층의 수에 따라 이름이 바뀐다
- 보통 4개
    - (1) Presentation Layer
    - (2) Business Layer(Domain Layer)
    - (3) Persistence Layer(Data Access Layer)
    - (4) Datavase Layer

### Presentation Layer
- 사용자와의 상호작용 처리에 관심사를 둔 계층
- View, Controller 등

### Business Layer(Domain Layer)
- 비즈니스 로직을 수행하는 것이 주 관심사
- Persistence Layer에서 데이터를 가져와서  비즈니스 로직을 수행하고 Presentation Layer로 전달
- Service, Domain Model 등

### Persistence Layer(Data Access Layer)
- 데이터베이스에 접근하는데 관심사를 둔 계층
- 데이터베이스와 매핑되는 객체로서의 엔티티를 영구히 관리
- ORM, Repository, DAO 등

### Database Layer
- 실제 DB 서버가 운영되는 계층
- DB등

## 장점과 단점

### 장점
- 코드 가독성과 유지 보수성이 높음
- 모듈 교체가 용이(다른 계층에 부수효과가 적게 작용)
- 의존관계가 느슨하여 테스트에 용이

### 단점
- 도메인이 DB에 영향을 받을 수 있다
- DB가 소프트웨어의 핵심이 될 위험이 있다
- Persistence 계층과 Domain 게층의 구분이 모호해지는 지점이 발생할 수 있다
