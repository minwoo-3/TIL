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
- 구체적이지 않는 클래스를 의미
- `abstract`를 클래스 앞에 써서 정의한다
- 미완성 추상 메서드를 넣을 수 있다
    - 추상 메서드 : 내용이 구현 되지 않은 메서드
- 객체를 생성 할 수 없다
- 상속받는 클래스가 추상 메서드를 구현한다

## super와 부모생성자
#### 부모 생성자
- 클래스가 인스턴스화 될때 생성자의 부모 생성자부터 실행된다

#### super
- 부모를 가르키는 키워드
    - this는 자신을 나타낸다
- 생성자의 부모 클래스가 기본 생성자가 아닌 경우 super()로 인수를 입력해야한다

## 오버라이딩
- 부모클래스의 메서드를 자식 클래스에서 재정의하여 사용하는 것
- 부모 클래스의 메서드가 사라지진 않는다
    - `super()`를 사용하면 부모 메서드도 호출 가능 하다
```
 public class Bus extends Car{
        public void run(){
            **super.run();**  // 부모의  run()메소드를 호출 
            System.out.println("Bus의 run메소드");
        }
    }
```

## 클래스 형변환
- 부모타입으로 자식 객체를 참조 할 수는 있으나 부모의 메서드만 사용 가능하다
```
public class BusExam{
        public static void main(String args[]){
            Car car = new Bus();
            car.run();
            //car.ppangppang(); // 컴파일 오류 발생

            Bus bus = (Bus)car;  //부모타입을 자식타입으로 형변환 
            bus.run();
            bus.ppangppang();
        }
    }
```
- 하지만 위와 같이 형 변환을 하면 자식 클래스의 메서드도 사용 할 수 있게 된다

## 인터페이스
- 인터페이스 : 서로 관계가 없는 물체들이 상호작용하기 위해 사용하는 장치나 시스템
    - 어떤 기능을 가질지 정의만 한다
    - 추상 메서드와 상수를 정의 할 수 있다
```
public interface TV{
        public int MAX_VOLUME = 100;
        public int MIN_VOLUME = 0;

        public void turnOn();
        public void turnOff();
        public void changeVolume(int volume);
        public void changeChannel(int channel);
    }
```
- 인터페이스를 구현하는 클래스에서 `implements` 키워드 사용한다
- 인터페이스의 메서드를 하나라도 구현하지 않으면 추상클래스가 된다
- 참조변수의 타입으로는 인터페이스를 사용할 수 있고 인터페이스가 가지고 있는 메서드만 사용할 수 있다

#### default method
- `default`키워드로 선언하면 인터페이스에서도 메서드를 구현 할 수 있다
- default메서드도 오버라이딩 할 수 있다

## 내부 클래스
- 클래스 안에 클래스

#### 내부 클래스 형태
- 중첩클래스(인스턴스 클래스) : 클래스 안에 인스턴스 변수
- static 클래스 : 내부 클래스가 static으로 정의된 경우
- 지역 (중첩) 클래스 : 메서드 안에 클래스를 선언한 경우
- 익명 (중첩) 클래스 : 생성자 다음에 중괄호로 열고 닫아 클래스를 상속받는 이름 없는 객체를 만드는 경우

## 예외 처리

### 예외 처리 문법
- try : 수행할 코드 예외 발생 가능성이 있는 블록
- catch : 예외 처리 블록
- finally : 무조건 실행되는 블록

### Throws
- 예외를 호출한 쪽에서 처리하도록 넘긴다
- Exception을 발생 시킬 수도 있다

### 사용자 정의 Exception
- checked Exception : Exception 클래스를 상속 받아 정의
    - 반드시 오류를 처리 해야하는 Exception
- unChecked Excoption : RuntimeException 클래스를 상속 받아 정의
    - 예외 처리 하지 않아도 커파일 오류 안일어남