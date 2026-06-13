# JPA
- Java Persistence API의 자바에서 사용되는 ORM 인터페이스의 모음이다

## ORM
- 객체를 RDB(관계형 DB)와 자동으로 연결해주는 것이다

### 장점
- SQL문을 안쓰고 Method로 DB조작 가능
- 코드 가독성 향상
### 단점
- 프로젝트 규모가 클때 속도 저하가 발생할 수 있음
- 복잡한 Query는 결국 튜닝 필요

## JPA
- Java에서 사용하는 ORM 기술 표준 인터페이스 모음
- 자바에서 RDB 사용하는 방식을 정의한 인터페이스

### 예시
```
// Album 객체저장
jpa.persist(album);
```
Album 클래스를 저장

```
INSERT INTO ITEM (ID, NAME, PRICE) .....
INSERT INTO ALBUM (ARTIST) .....
```
자동으로 SQL문으로 바뀜

## JPA 구현체 - Hibernate
- 구현체 중 하나로 JPA 표준을 따르는 ORM 프레임 워크
- JDBC 사용
- 쿼리 없이도 데이터를 읽고 쓸 수 있음
- HQL, JPQL, Criteria API 사용 가능

### HQL
- DB 테이블을 객체 지향적으로 조작할 수 있게 해주는 하이버네이트 쿼리 언어
- "객체 중에서 무슨 필드가 몇인 애들 가져와" 같은 구조

```
SELECT u FROM User u WHERE u.age > 20
```
- User 객체 중 age가 20 이상인 애들 조회

## JPQL
- SQL과 비슷하지만 객체 중심으로 데이터 처리
- HQL과 비슷하지만 만든 사람과 지원 범위차이

## Criteria API
- JPQL을 빌더 클래스로 사용하여 타입-세이프한 쿼리
- JPQL를 문자열 말고 자바 코드로 쓰는 방식

- 원래 방식: 
```
em.createQuery(
    "SELECT m FROM Member m WHERE m.age > 20"
);
```
- Criteria API 방식:
```
CriteriaBuilder cb = em.getCriteriaBuilder();
CriteriaQuery<Member> cq = cb.createQuery(Member.class);

Root<Member> m = cq.from(Member.class);

cq.select(m)
  .where(cb.gt(m.get("age"), 20));

List<Member> result = em.createQuery(cq).getResultList();
```

## QueryDSL
- Java 언어를 기반으로 한 세이프한 쿼리 빌터 프레임 워크
- SQL/JPQL을 문자열이 아닌 자바코드로 쓰게 해줌

## 1차 캐시
- 영속성 컨텍스트(엔티티 객체를 관리해주는 공간) 내부에 존재
- DB에서 가져온 객체를 잠깐 보관해두는 곳
- 트랜잭션이 끝나면 1차 캐시 사라짐
- 1차 캐시에 객체가 있으면 굳이 DB에 접근하지 않아도 돼서 편리함

## 2차 캐시
- 자주 쓰는 객체를 저장해서 DB 접근을 크게 줄이는거
- 복제본을 반환해준다
- 앱 전체가 범위여서 트랜잭션이 끝나도 사라지지 않는다

## 프록시
- JPA와 Hibernate에서 사용되는 진짜 객체 대신 동작하는 가짜 객체
### 특징
- ### 데이터베이스 조회 지연: 
  - 데이터베이스의 조회를 미루도 프록시 객체를 반환함
- ### 프록시 객체의 구조:  
  - 프록시 객체는 엔티티 클래스를 상속받고 실제 엔티티 객체를 참조한다
- ### 프록시 객체의 초기화: 
  - 프록시 객체는 처음 사용할 때 한 번만 초기화 프록시 객체는 실제 엔티티 객체의 데이터에 접근할 수 있게 된다
- ### 프록시와 타입 비교: 
  - 프록시 객체는 실제 엔티티와 동일한 인터페이스를 구현 하기 때문에 타입을 비교 할 때 instanceof 연산자를 사용한다
- ### 영속성 컨텍스트와 프록시:
  - 영속성 컨텍스트에 이미 엔티티가 있으면 `em.getReference()`는 프록시가 아닌 실제 객체 반환
  - 준영속 상태(살아는 있지만 JPA에서 관리하지 않는 상태)에서 프록시를 초기화 하면 예외 발생

## LAZY 로딩과 프록시
- 프록시 생성:
  - LAZY(필요할때 연관 객체를 가져오는 것)로 연관관계가 설정되는 실제 엔티티가 아닌 프록시 객체로 로드된다
- 예시:
```
@Entity
public class Member {
    @Id
    @GeneratedValue
    private Long id;

    private String name;

    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "team_id")
    private Team team;

    // getters and setters
}

@Entity
public class Team {
    @Id
    @GeneratedValue
    private Long id;

    private String name;

    @OneToMany(mappedBy = "team")
    private List<Member> members = new ArrayList<>();

    // getters and setters
}
```
- Member 엔티티 로드될때 team은 프록시 객체로 로드
```
EntityManager em = emf.createEntityManager();
EntityTransaction tx = em.getTransaction();
tx.begin();

Member member = em.find(Member.class, memberId);
Team team = member.getTeam(); // 이 시점에는 프록시 객체가 로드됨
String teamName = team.getName(); // 이 시점에 실제 DB 조회 발생

tx.commit();
em.close();
```
- 필요해져서 실제 DB 조회
- 불필요한 조회를 줄여서 성능 향상

## 프록시 초기화 확인
- 초기화 확인 코드
```
PersistenceUnitUtil persistenceUnitUtil = emf.getPersistenceUnitUtil();
boolean isInitialized = persistenceUnitUtil.isLoaded(member.getTeam());
```
- 강제 초기화
```
org.hibernate.Hibernate.initialize(member.getTeam());
```

## 프록시 객체의 작동 방식

- 프록시 객체 생성:
  - 연관관계 엔티티를 호출하면 처음엔 실제 엔티티가 아니라 프록시 객체가 반환
  - 프록시 객체는 실제 데이터를 갖고 있지 않고 DB에서 데이터를 로드하는 로직만 포함

- 초기화 전 상태:
  - 프록시 객체는 이 시점에서 DB에 로드 안됨
  - 프록시 객체는 DB 접근이 필요하기 전까지 로드 안됨

- 실제 데이터 접근 시점:
  - `.getName()`등을 호출하면 실제 데이터를 로드
  - 이 시점에서 DB쿼리 실행 프록시 객체에 로드됨 ->`초기화`

- 초기화 후 상태:
  - 초기화 후엔 프록시 객체는 실제 데이터를 갖게 됨 이후 데이터 접근은 이미 로드된거 사용

### 프록시와 즉시 로딩
- 가급적 지연 로딩만 사용

- #### 즉시 로딩(Eager Loading):
  - 엔티티와 연관된 모든 데이터 즉시 싹 다 가져오기

- #### 지연 로딩(Lazy Loading):
  - 연관된 엔티티의 데이터를 실제 필요할때만 DB에서 가져오기

## CASCADE(영속성 전이)
- 특정 엔티티를 영속 상태로 만들 때 연관된 객체도 모두 영속 상태로 만들 때 사용하는 기능

### 종류
- <b>ALL</b> : 모든 영속성 전이 옵션을 적용

- <b>PERSIST</b> : 영속 상태로 전이

- <b>REMOVE</b> : 삭제 상태로 전이

- <b>MERGE</b> : 병합 상태로 전이

- <b>REFRESH</b> : 새로 고침 상태로 전이

- <b>DETACH</b> : 준영속 상태로 전이

## 고아 객체(Orphan Removal)
- 부모 엔티티와 연관 관계가 끊어진 자식 엔티티 JPA가 자동으로 삭제하는 기능을 제공한다
- `orphanRemoval = true` : 고아 객체 제거

```
@Entity
class Parent {
    @Id @GeneratedValue
    private Long id;

    private String name;

    @OneToMany(mappedBy = "parent", cascade = CascadeType.ALL, orphanRemoval = true)
    private List<Child> children = new ArrayList<>();

    public void addChild(Child child) {
        children.add(child);
        child.setParent(this);
    }

    public void removeChild(Child child) {
        children.remove(child);
        child.setParent(null);
    }

    // getters and setters
}

@Entity
class Child {
    @Id @GeneratedValue
    private Long id;

    private String name;

    @ManyToOne
    @JoinColumn(name = "parent_id")
    private Parent parent;

    // getters and setters
}
```
- 부모 엔티티에서 자식 엔티티 끊어버리면 고아 객체로 감지돼서 삭제됨

## JPA 쿼리 메서드
- 메서드명을 규칙에 맞게 작성하면 자동으로 쿼리가 생성되는 기능

### 쿼리 Subject 키워드
- 메서드 처음 시작 위치에 오는 키워드
- 어떤 작업인지, 어떤 데이터 타입 리턴할지 결정됨
- 관례 아니고 규칙

#### 종류
- findBy : 조회하는 결과가 여러 개면 List로 받고, 무조건 하나면 하나의 객체로 받는다

- findFirst : 제일 처음 데이터 1개 리턴

- existBy : boolean 타입으로 리턴

- countBy : int/long 등으로 해당되는 데이터 수 리턴

- deletBy : void 타입 혹은int/long 등으로 삭제된 데이터 수 리턴

### 메서드명 내 키워드
- 메서드명 By 뒤에 작성되는 키워드로 SQL의 Where절 내 명령어와 같은 역할