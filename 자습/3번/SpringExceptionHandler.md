# Spring ExceptionHandler

## @ExceptionHandler
- 스프링 프레임워크 어노테이션
- 예외를 처리하는 메서드를 지정

### 특정 예외 처리:
- 특정 예외를 인자로 받아 그 예외가 발생했을 때 실행될 메서드를 정의

### 컨트롤러 범위:
- `@ControllerAdvice`와 함께 사용하면 앱 전체의 예외를 전역적으로 처리

### 맞춤형 응답:
- 발생한 예외에 대해 사용자 정듸 응답을 생성 할 수 있음
- HTTP 상태코드, JSON 형식의 에러 메세지

### 예외의 계층 구조 처리:
- 특정 예외에 대한 핸들러가 없으면 부모 클래스의 예외 핸들러 호출

### 간편한 예외 관리:
- `@ExceptionHandler`를 사용하면 예외처리 로직을 분리하여 코드 가독성을 높임

## @ControllerAdvice
- `@Controller`와 `handler`에서 발생하는 에러들을 모두 잡아준다
- 안에서 `@ExceptionHandler`를 사용해서 에러를 잡을 수 있음

```
@ControllerAdvice
public class ExceptionHandlers {

    @ExceptionHandler(FileNotFoundException.class)
    public ResponseEntity handleFileException() {
        return new ResponseEntity(HttpStatus.BAD_REQUEST);
    }


}
```

## @RestControllerAdvice:
- `@ControllerAdvice`와 `@ResponseBody`를 가지고 있다
- `@Controller`처럼 작동하며 `@ResponseBody`를 통해 객체를 리턴