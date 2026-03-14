변수
=
변수란?
-
변수란 값을 저장할 수 있는 상자(공간)이다<br>
변수의 종류는 5가지로 나뉜다

<h3> 변수의 종류 </h3>

  + int : 정수
  + double : 실수
  + boolean : 참 거짓 판단
  + char : 문자 하나
  + String : 문자열(여러 문자)
  + long :  정수
  + short : 정수(범위 짧음)
  + byte : 정수(범위 짧음)
  + float : 실수(정확도 낮음)

<br>
각 각 역할에 맞지 않는 값을 넣으면 오류가 발생한다<br> 
* short, byte, char, float은 잘 사용하지 않는다

<h2>변수의 사용</h2>

<h4>Var1</h4>
<pre>package variable;

public class Var1 {

    public static void main(String[] args) {
        System.out.println(10);
        System.out.println(10);
        System.out.println(10);
    }
}</pre>

<h4>실행 결과</h4>
<pre>
10
10
10</pre>

위 코드의 결과 값 모두 20으로 바꾸려면 어떻게 해야할까?<br>
10을 모두 찾아서 일일히 바꿔야한다<br>
이런 귀찮은 일을 방지하기 위해서 존재하는게 변수다<br>
만약코드에 변수를 사용한다면
<h4>변수를 사용한 예</h4>
<pre>package variable;

public class Var2 {
    public static void main(String[] agrs) {
        int a; //변수 선언
        a = 20;

        System.out.println(a);
        System.out.println(a);
        System.out.println(a);
    }
}
</pre>

<h4>실행 결과</h4>
<pre>20
20
20
</pre>
위 코드처럼 변수를 사용하게 되면 코드를 1번만
수정해도<br>원하는 결과값을 얻을 수 있게된다<br>
이처럼 코드를 짤 때는 어딘가에 값을 보관해두고 언제든 <br> 
 값을 꺼내서 읽을 수 있는 변수가 중요하다<br>

 <h2>변수 명명 규칙</h2>
 변수의 이름을 지을때는 아래와 같이 따라야할 규칙과 관례가 있다<br>
*규칙은 필수지만 관례는 지키면 좋은 것 정도이다
 <h3>규칙</h3>

+ 변수의 이름은 숫자로 시작할 수 없다 
+ 이름엔 공백이 들어 갈 수 없다
+ 예약어를 이름으로 사용할 수 없다
+ 변수 이름에는 영문자, 숫자, 달러기호 <br> 또는 밑줄만 사용할 수 있다

<h3>관례</h3>

+  소문자로 시작하는 낙타 표기법
+  클래스의 첫글자는 대문자 나머진 소문자
+  상수는 모두 대문자
+  패키지는 모두 소문자

<h2>에제</h2>
<h3>문제1</h3>
다음 코드에 반복해서 나오는 숫자 4, 3
을 다른 숫자로 한번에 변경할 수 있도록 다음을 변수 `
num1`
, `
num2`
를 사용하
도록 변경해보세요.

<h4>VarEx1</h4>
<pre>package variable.ex;
public class VarEx1Question {
    public static void main(String[] args) {
        System.out.println(4 + 3);
        System.out.println(4 - 3);
        System.out.println(4 * 3);
    }
}</pre>

<h4>VarEx - 답</h4>
<pre>package variable.ex;

public class VarEx1 {

    public static void main(String[] args) {

        int num1 = 4;
        int num2 = 3;

        System.out.println(num1 + num2);
        System.out.println(num1 - num2);
        System.out.println(num1 * num2);

    }
}</pre>

<h3>문제2</h3>

다음과 같은 작업을 수행하는 프로그램을 작성하세요.
1. 변수 
`
VarEx2
num1
`
1. 변수 
`
num2
라고 적어주세요.
`
을 선언하고, 이에 10을 할당하세요.
`
`
를 선언하고, 이에 20을 할당하세요.
1. 두 변수의 합을 구하고, 그 결과를 새로운 변수 
2. `
sum
`
변수의 값을 출력하세요.

<h4>VarEx2 - 답</h4>
<pre>package variable.ex;

public class VarEx2 {
public static void main(String[] args) {
int num1 = 10;
int num2 = 20;
int sum = num1 + num2;
System.out.println(sum);
    }
}</pre>

<h3>문제3</h3>
long 타입의 변수를 선언하고, 그 변수를 10000000000(백억)으로 초기화한 후 출력하는 프로그램을 작성하세요.
boolean
타입의 변수를 선언하고, 그 변수를 true로 초기화 한 후 출력하는 프로그램을 작성하세요.

<h4>VarEx3 - 답</h4>

<pre>package variable.ex;

public class VarEx3 {

    public static void main(String[] args) {
        long longVar = 10000000000L;
        System.out.println(longVar);

        boolean booleanVar = true;
        System.out.println(booleanVar);

    }
}</pre>