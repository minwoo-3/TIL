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