# OOP

> 객체 지향 프로그래밍에서 객체란 무엇일까? 객체는 속성과 기능을 가지는 하나의 프로그램 단위이다. 이를 현실과 비교 한다면, 자전거의 경우 속성는 바퀴, 체인, 브레이크, 기어이고, 기능은 가기, 멈추기가 있을 수 있다. 또한, 객체 지향을 이해하기 위한 다양한 특성을 들을 알아보고자 한다.:smiley:



# Ⅰ. 클래스

 객체를 만들기 위한 틀이라고 생각하면 이해하기 쉽다. 예를 들어 자동차 클래스로 부터 a, b, c라는 객체를 생성하지만 각각의 객체마다 다른 속성를 지정할 수 있다.  이와 같이 클래스는 속성(멤버 변수)와 기능(메소드)로 구성된다.



```java
public class Car {
    public String name;
    public String color;
    public int price;
    
    public Car() {
        System.out.println("생성자 호출");
    }
    
   	public void run() {
        System.out.println("go go");
    }
    
    public void stop() {
        System.out.println("stop stop");
    }
                
}
```

 클래스의 예제는 위의 Car와 같다. 클래스를 정의 할 때, 클래스 이름과 동일한 생성자를 지정해주면, 최초에 해당 클래스의 객체를 생성할 때 처리하고자 하는 작업을 지정할 수 있다. 또한 멤버 변수와 메소드를 구현하여 객체 생성 후 값을 입력하거나 처리할 수 있다.



## 1. 생성자

```java
public class Car {
    public String name;
    public String color;
    public int price;
    
    public Car() {
        System.out.println("기본 생성자 호출");
    }
    
    public Car(String name, String color, int price) {
        System.out.println("인자를 받는 생성자 호출");
    }
    
	/* 생략  */               
}
```

 생성자는 객체를 생성 할 때, `Car a = new Car();`과 같이 인자로 아무것도 지정하지 않을 수 있지만, 위의 코드와 같이 `Car b = new Car(good, black, 10000);`으로 인자를 전달 할 수 도있다. 위의 코드에서 알 수 있듯이 생성자는 기본 생성자 이외에 다른 생성자를 선언해 두어도 된다.

 만약 생성자를 선언하지 않으면, 컴파일러가 자동으로 아무 기능을 하지 않는 default 생성자가 생성되어 동작하게 된다.   



## 2. 소멸자

```java
@Override
protected void finalize() throws Throwable {
    super.finalize();
}
```

 직접 명시적으로 작성하는 경우는 드물다. 하지만 소멸되는 것이 finalize를 통해 이루어진다는 것은 알아둘 필요가 있다. `System.gc()`를 호출하면 강제적으로 GC를 실행하지만, 객체들의 메모리 반환이 즉시 이루어지는 것은 아니며 최대한 빠른 시간 내에 반환한다. 하지만 이 역시 사용하는 일은 거의 없다.



## this 키워드

```java
public class Car {
    public String name;
    public String color;
    public int price;
        
    public Car(String name, String color, int price) {
        this.name = name;
        this.color = color;
        this.price = price;
    }
    
	/* 생략  */               
}
```

 this는 현재 클래스 자체를 가르키게 된다 따라서 위의 코드에서 `this.name`는 Car 클래스의 name이며, name은 객체 생성 시 전달 받은 값이다. 즉, 현재 객체를 가르킬 때 this를 사용한다고 생각하면 쉽다.



## 3. 메소드

 메소드는 다른 언어에서 보면 함수와 같다. 자바의 메소드는 다른 언어와 유사하게 반환 타입, 함수 이름, 매개 변수(Parmeter)로 선언하게 된다.



### 가. Overloading

 Overloading은 중복된 메소드를 매개 변수를 달리하여 재사용할 수 있는 개념을 말한다. 아래의 간단한 예제를 보면 쉽게 이해할 수 있다.



```java
public class Car {
    public String name;
    public String color;
    public int price;
    
    public Car() {
        System.out.println("기본 생성자 호출");
    }
    
    public void run() {
        System.out.println("go go");
    }
    
    public void run(int distance) {
        System.out.println("move " + distance);
    }
}
```

 따라서 클래스 Car의 a 객체를 생성 후 `a.run();` `a.run(10);`과 같이 매개 변수에 따라 다른 기능을 수행할 수 있다. 또한 Overloading의 조건으로 매개변수의 타입이 달라야 한다는 제한이 있다.



## 4. 접근 제한자

 접근 제한자의 종류는 다음과 같다. 제한자를 통해 데이터 은닉을 할 수 있다.

* **public** : 모든 접근을 허용
* **protected** : 같은 패키지에 있는 객체와 상속관계의 객체들만 허용
* **default**  : 같은 패키지에 있는 객체들만 허용
  * class나 변수 앞에 아무것도 적지 않는 것을 말한다.
* **private** : 현재 객체 내에서만 허용



### 가. 데이터 은닉

 클래스 내의 멤버 변수를 private로 지정하면, 데이터를 은닉할 수 있다. 따라서 클래스 내의 멤버 값을 변경하거나 접근하기 위해서는 get, set과 같은 메소드를 구현하여 활용하여야 한다. 또한, 주민번호, 학번과 같이 변경되면 안되는 값인 경우에는 private로 지정하여 접근할 수 없도록 하여야 한다.



## 5. 패키지

 프로젝트에 따라 다수의 클래스가 생성되면 관리하는 것이 힘들어 진다. 따라서 관련된 클래스를 하나의 패키지에 소속되도록 하여 관리를 하기가 용이해 진다. (실제 src 디렉토리에 가면 패키지는 폴더로 구성되어 있다.)

 다른 패키지에 소속된 클래스들을 사용하고자 할 때는 import를 하여 사용하면 된다. 예를 들어 `java.uitl.*`과 같이 해당하는 패키지의 모든 클래스를 사용할 수 도 있고, 특정 클래스만 지정하여 사용할 수 도 있다.



---



# Ⅱ. 객체 생성과 삭제

 객체의 생성은 `Car a = new Car();`과 같이 생성되게 된다. 여기서 a는 객체자체가 아닌 레퍼런스로 생성된 객체의 주소를 가르키게 된다.  특정 객체의 레퍼런스가 없으면, GC(Garbage Collection)를 통해 해당 객체의 메모리 할당을 해제 하게 된다. 이와 같은 과정이 객체가 삭제 되는 과정이다.



```java
class Car {
    private int price;
    
    void run() {
        System.out.println()
    }
}

public class Test {
    public static void main(String args[]) {
        Car a = new Car();
        Car b = new Car();
    }
}
```

 위와 같이 Car 클래스에 대한 객체 a, b를 생성하면 서로 다른 주소를 레퍼런스하게 된다. 만약 객체가 더 이상 사용되지 않는 경우 `a = null;`과 같이 선언하게 되면, 해당 객체의 레퍼런스가 없으므로 GC에 의해 메모리 할당이 해제 된다.