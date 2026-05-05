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