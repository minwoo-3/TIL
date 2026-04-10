# Spring

## Entity
- 실제 DB와 매칭될 클래스
- 데이터베이스 테이블의 각 행을 표현
- 데이터베이스 연산을 추상화한다 이를 통해 데이터베이스와 상호작용 가능

```
@Entity
public class User {

    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    @Column(name = "user_sq", unique = true)
    private long userSq;
    
}
```

## Controller
- Spring MVC패턴의 화면(View)과 모델(Model)을 연결시킨다