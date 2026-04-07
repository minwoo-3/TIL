## Scanner

scanner를 사용하면 값을 입력 받을 수 있다
```
import java.util.scanner //scanner 불러오기
Scanner scanner = new Scanner(System.in);//new를 사용해서 Scanner를 만든다
```
* `scanner.nextLine()` : 엔터(`\n`)를 입력할 때까지 문자를 가져온다
* `scanner.nextInt()` : 입력을 `int`형으로 가져온다 정수 입력에 사용한다
* `scanner.nextdouble()` : 입력을 `double`형으로 가져온다 정수 입력에 사용한다

## 배열
배열은 같은 타입의 변수를 하나로 묶어서 <br>
반복해서 선언하고 반복해서 사용하는 문제를 해결한다

```
int[]  배열 변수 이름; //배열 변수 선언
배열 변수 이름 = new int[5]; //배열 생성
```
* `new int[5]`로 배열을 생성하면 배열의 크기만큼 메모리를 확보한다

### 2차원 배열
```
// 2x3 2차원 배열, 초기화
int[][] arr = {
{1, 2, 3},
{4, 5, 6}
}
```
* 행과 열로 이루어져있다

### 인덱스
배열에서 값을 불러 올때 사용하는 수다
```
//변수 값 대입
students[0] = 90;
students[1] = 80;
//변수 값 사용
System.out.println("학생1 점수: " + students[0]);
System.out.println("학생2 점수: " + students[1]);
```
* 0부터 시작한다

### 기본형과 참조형
* 기본형은 계속 사용하던 `int` `double` `long` `boolean`등으로 <br>
사용하는 값을 직접 넣는다
* 참조형은 `int[] students`처럼 데이터에 접근하기 위해 주소(참조)를 저장하는 것이다

## 향상 for문
```
for (변수 : 배열 또는 컬렉션) {
// 배열 또는 컬렉션의 요소를 순회하면서 수행할 작업
}
```
* 각각의 요소를 탐색한다는 의미로 for-each문이라고도 많이 부른다
* 증가하는 인덱스 값을 직접 사용해야하는 경우에는 향상된 for문을 사용할 수 없다

## 메서드
자바에서의 함수이다

```
//add 메서드 정의
public static int add(int a, int b) {
System.out.println(a + "+" + b + " 연산 수행");
int sum = a + b;
return sum;
}
```
이런식으로 메서드를 만들면 긴 연산을 간결하게 여러번 쓸 수 있다
```
int sum1 = add(5, 10);
int sum2 = add(15, 20); // 메서드 사용
```
### 용어 정리
* 인수 : 메서드를 사용할때 넘기는 수를 말한다
* 매개변수 : 파라미터라고도 하면 정의 할때 사용하는 수를 말한다