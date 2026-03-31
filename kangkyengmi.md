## 클래스
* 객체를 만들기 위한 틀이다
* 붕어빵틀, 자동차틀 같은 것
* 메모리의 위치를 저장하는 참조타입이다 
### 객체
* 만들어진 자동차, 붕어빵 같은거다
### 클래스 선언
```
public class 클래스명 {

}
```
### 객체 생성
```
public class CarExam{
        public static void main(String args[]){
            Car c1 = new Car();
            Car c2 = new Car();
        }
    }
```
* 이렇게 만들어진 객체를 인스턴스라고도 한다.

## 필드
- 객체의 이름 번호등과 같은 속성 개념이다
  
```
    public class Car{
        String name;    
        int number;
    }
```
이런식으로 클래스를 만들면

```
Car c1 = new Car();
    Car c2 = new Car();

    c1.name = "소방차"; 
    c1.number = 1234;
    c2.name = "구급차"
    c2.number = 1004;
    System.out.println(c1.name); 
    System.out.println(c1.number);

    String name = c2.name;
```
다른 클래스에서 참조 가능

## 메서드
- 필드가 물체의 상태라면, 메서드는 물체의 행동이다
- 입력값을 받아 결과를 도출하는 함수와 같은 개념이다
- 입력값을 매개변수, 결과값을 리턴값이라고 한다


### 메서드 종류

- #### 매개변수X 리턴값X
```
    public void method1() {
            System.out.println("method1이 실행됩니다.");
    }
```

- #### 정수를 받아들인 후, 리턴하지 않는 메서드

```
public void method2(int x){
            System.out.println(x + " 를 이용하는 method2입니다.");
        }
```

- #### 아무것도 받아들이지 않고, 정수를 반환하는 메서드

```
public int method3(){
        System.out.println("method3이 실행됩니다.");

        return 10;
    }
```

- #### 정수를 2개 매개변수로 받고, 아무거도 반환하지 않는 메서드

```
public void method4(int x, int y){
        System.out.println(x + "," + y + " 를 이용하는 method4입니다.");
    }
```

- #### 정수를 한개 받아들인 후, 정수를 반환하는 메서드

```
public int method5(int y){
        System.out.println(y + " 를 이용하는 method5입니다.");
        return 5;
    }
```

## String 클래스 메서드

- #### 문자열 길이 구하기

```
 System.out.println(str.length());
```

- #### 문자열 붙히기

```
String str = new String("hello");

    System.out.println(str.concat(" world")); 
    System.out.println(str);
```

- #### 문자열 자르기

```
 System.out.println(str.substring(1, 3));
    System.out.println(str.substring(2));
```

## scope와 static

### scope
- 변수들의 사용 가능 범위
- 변수가 선언된 블럭이 그 범위이다
```
    public class ValableScopeExam{

        int globalScope = 10;

        public void scopeTest(int value){   
            int localScope = 10;
            System.out.println(globalScope);
            System.out.println(localScpe);
            System.out.println(value);
        }
    }

```
위에 globalScope는 사용 범위가 클래스 전체이다
하지만 main 메서드에선 사용 할수 없다 이유는 static에 있다

```
public static void main(String[] args) {
            System.out.println(globalScope);  //오류
            System.out.println(localScope);   //오류
            System.out.println(value);        //오류  
        }   
```
staic한 메서드에건 static하지 않은 필드를 사용 할 수 없다

### static
- 위에 main처럼 static이라는 키워드로 정의된 메서드를 static한 메서드라고 한다
- Class가 인스턴스화 되지 않아도 사용할 수 있게 해준다
- static하게 선언된 변수는 값이 한 공간의 저장되므로 인스턴스가 여러개여도 변수가 하나다

```
v1.staticVal = 10;
    v2.staticVal = 20; 

    System.out.println(v1.statVal);  //20 이 출력된다. 
    System.out.println(v2.statVal);  //20 이 출력된다. 
```
- 인스턴스 변수 : 변수가 인스턴스와 함께 새로 생선되는 변수이다
- 클래스 변수 : 값을 저장할 수 있는 공간 하나를 공유하는 변수이다

## 열거형
- 원래는 상수를 사용하였지만 원하지 않는 값이 들어와도 컴파일 오류가 발생하지 않는 문제를 해결하기 위해 열거형이 추가 되었다
- #### 열거형 정의

```
enum 이름{
        값1, 값2;
    }
```
- #### 열거형 사용
  
```
Gender gender2;

    gender2 = Gender.MALE;
    gender2 = Gender.FEMALE;
```

## 생성자
- 모든 클래스는 인스턴스(객체)화 될때 사용한다

#### 특징
- 리턴타입이 없다
- 생성자는 만들지 않으면 매개변수가 없는 생성자가 자동으로 생성된다
- 기본 생성자 : 매개변수가 없는 생성자

#### 역할
- 객체가 될 때 필드를 초기화 한다
```
    public class Car{
        String name;
        int number;

        public Car(String n){
            name = n;
        }
    } //String 값을 초기에 받아야 함
```

## this
- 객체 자신을 참조하는 키워드
- 변수명이 같은게 있을때 무엇을 의미하는지 구분하게
- 클래스 안에서 가지고 있는 매서드를 사용할 때도 사용한다
    -  this.메서드명()


## 오버로딩
#### 메서드 오버로딩
- 매개변수의 수, 타입이 다르면 동일한 이름의 메서드를 여러개 정의할 수 있다

```
class MyClass2{
        public int plus(int x, int y){
            return x+y;
        }

        public int plus(int x, int y, int z){
            return x + y + z;
        }

        public String plus(String x, String y){
            return x + y;
        }
    }
```
- 매겨변수가 달라야 한다
- 변수명은 관계 없다

```
public MethodOverloadExam{
        public static void main(String args[]){
            MyClass2 m = new MyClass2();
            System.out.println(m.plus(5,10));
            System.out.println(m.plus(5,10,15));
            System.out.println(m.plus("hello" + " world"));
        }
    }
```
- 메서드의 인자에 따라 다른 메서드가 호출된다

#### 생성자 오버로딩
- 매개변수의 수와 타입이 다르면 같은 이름의 생성자를 여러개 선언할 수 있다

```
public class Car{
        String name;
        int number;

        public Car(){

        }

        public Car(String name){
            this.name = name;
        }

        public Car(String name, int number){
            this.name = name;
            this.number = number;
        }
    }
```
- 메서드와 같이 매개변수가 다르면 여러가지를 생성가능
- 사용할땐 매개변수에 따라 다른 생성자 호출

##### this()
- 자기 자신의 생성자를 호출 할 수 있다

```
 public Car(){
        this("이름없음", 0);
    }
```

## 패키지
- 관련있는 클래스 또는 인스턴스들을 묶어 놓은 묶음이다
- 패키지끼리는 충돌을 막기위해 나눠져 있다
    - 다른 패키지의 클래스를 사용할땐 import를 쓰면 된다

## 상속
- 부모 클래스의 것을 자식 클래스에 물려주는것
    - 소방차는 자동차다
    - 노트북은 컴퓨터의 한 종류다
    -  is a 관계 혹은 kind of 관계라고 한다
```
public class 클래스명 extends 부모클래스명
```
- 부모클래스의 메서드는 자식클래스가 사용할 수 있다
- 자식클래스의 메서드는 부모 클래스가 사용할 수 없다

## 접근제한자

#### 캡슐화
- 관련된 내용을 모아서 가지고 있는 것

#### 접근제한자 종류
- public
    - 모두 접근
- protected
    - 자기 자신, 같은 패키지, 서로 다른 패키지여도 상속 받은 자식 클래스는 접근 가능
- privite
    - 자신만 접근 가능
- 접근제한자 작성X default로 지정
    - 자신과 같은 패키지만 접근 가능

## 추상클래스

