# Mockito
- 개발자가 동작을 제어할 수 있는 Mock(가짜) 객체를 지원하는 테스트 프레임워크

- Mock: 진짜 객체와 비슷하지만 물리적으로 다르고 모든 상호작용은 기억하는 객체

- Stubbing: Mock의 특정 메서드 호출과 응답을 정의하는 것
    - when: 스터빙 조건
    - thenReturn: 스터빙한 메서드 호출 후 리턴 값 정의
    - thenThrow: 스터빙한 메서드 호출 후 어떤 Exception을 던질지 정의

## 사용 이유
- 구현 되지 않은 객체나 메서드를 테스트 하려고 할 때 Mock 객체를 미리 생성해서 테스트를 할 수 있기 때문이다

## Mockito Annotaion
- `@Mock`: 
    - Mock 객체를 생성
    - Stubbing 하지 않으면 null을 반환하여 테스트 불가
```
@Mock
List<Sring> mokesList
```
- `@Spy`: 
    - 메서드 실행 시 Stubbing 안하면 기존 로직을 실행 하는 mock 객체 생성

- `@InjectMocks`: DI(의존성 주입)를 `@Mock`이나 `@Spy`로 된 mock 객체를 자동으로 

- `@Captor`: 
    - 메서드에 전달된 인자를 가로채서 저장하기 위한 어노테이션
    - Verify 메서드로 메서드의 인자를 캡처할 수 있다
    - Mock 객체가 주입되어 내부적으로 객체의 메서드가 호출되는 경우에도 인자를 캡쳐 가능

    