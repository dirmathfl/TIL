# Polymorphism

> 다형성(Polymorphism)을 이해하기 위해서는 **상속, 인터페이스, 추상 클래스와 같은 기능들에 대해 정확히 이해할 필요**가 있다.  이와 같은 기능들은 하나의 클래스를 보다 다양하게 활용할 수 있게 해주며, 구현의 안정성을 높이거나, 효율적으로 사용할 수 있게 해준다. 따라서 자바에서 다형성이란 무엇인가? 라고 질문하였을 때 각 특성들로 인해 이렇다라고 명확히 설명할 수 있다면 자바 OOP에 대해 이해한 것이라고 생각된다.:+1:



# Ⅰ. 상속

 특정 클래스를 상속 받으면 상위 클래스의  정의된 기능을 구현하지 않고, 사용할 수 있으며, 상속한 클래스는 상위 클래스의 기능 이외에 다른 기능을 구현하여 사용할 수 있다.



## 1. 상속의 필요성

 기존의 클래스 중 오류 없이 검증된 클래스가 있다면, 비슷한 기능을 구현하고자 할 경우 처음 부터 끝까지 모두 구현하지 않고 상속을 받아, 필요한 기능 몇가지를 구현한다면 구현 속도를 높일 수 있을 뿐 아니라 오류도 줄일 수  있다. 간단한 클래스의 경우 모든 부분을 구현하는 것은 어려운 일이 아니지만, 복잡한 클래스의 경우 매번 구현하는 것은 번거로운 일이다.



## 2. 상속 구현

```java
public class A {
    public A() {
        System.out.println("A 생성자");
    }
    
    public void callA() {
        System.out.println("A - Call");
    }
}

public class B extend A {
    public B() {
        System.out.println("B 생성자")
    }
    
    public void callB() {
        System.out.println("B - Call");
    }
}

public class Test {
    public static void main(String args[]) {
        B child = new B();
        child.callA();
        child.callB();
    }
}
```

 위와 같이 특정 클래스를 상속 받고자 한다면, `extend` 키워드를 통해 클래스를 상속 받을 수 있다. A, B 클래스는 각각 생성자가 호출될 경우 메시지를 출력한다. 만약, **자식 클래스인 B를 생성할 경우 부모 클래스인 A의 생성자도 호출이 될까?** 당연하게도 부모 클래스가 생성되어야 해당 하는 기능들을 상속 받을 수 있기에, 부모 클래스 부터 먼저 생성된 후에 자식 클래스가 생성되게 된다.

  자바는 상속을 지원하는 다른 언어와 달리 **다중 상속을 지원하지 않는다.** 이 역시도 직관적으로 생각한다면 class C가 두 개의 클래스 A, B로 부터 상속을 받고 A, B에 동일한 메소드가 있다면 `super` 키워드를 통해 부모의 메소드를 사용하고자 할 때 A, B 중 어느 부모를 접근하여야 하는지에 대한 충돌이 발생하게 된다. 이에 자바에서는 해당 문제점을 일으키지 않도록, 다중 상속은 원천적으로 막았다.



## 3. 상속 특징

### 가. 메소드 오버라이드

```java
public class A {
    public A() {
        System.out.println("A 생성자");
    }
    
    public void go() {
        System.out.println("left side");
    }
}

public class B extend A {
    public B() {
        System.out.println("B 생성자")
    }
    
	@Override
    public void go() {
        System.out.println("left side");
    }
}
```

 상속을 받을 경우 부모 클래스를 상속하는 것만으로 자식 클래스에서 부모 클래스의 메소드를 사용할 수 있다.이와 달리 메소드 오버라이드는 위의 코드 처럼 **부모 클래스의 메소드를 자식 클래스에서 메소드의 기능을 변경하기 위해 재정의(Override)하는 것**을 말한다.



### 나. 부모 클래스의 활용

```java
public class A {
    public A() {
        System.out.println("A 생성자");
    }
    
    public void go() {
        System.out.println("left side");
    }
}

public class B extend A {
    public B() {
        System.out.println("B 생성자")
    }
    
	@Override
    public void go() {
        System.out.println("left side");
    }
}

public class Test {
    public static void main(String args[]) {
        A child = new B();
    }
}
```

 만약 하나의 부모 클래스에 여러 클래스들이 상속 받는 경우에는 자식 클래스들을 생성하기 위해, 모든 자식 클래스들의 클래수 변수로 생성하여야 할까? 자식 클래스들은 하나의 부모 클래스를 상속 받기에 부모 클래스의 클래수 변수를 자료형으로 지정하더라도 정상적으로 동작한다. 따라서 여러 개의 자식 클래스를 사용하고자 한다면, 부모 클래스의 클래스 변수 배열을 선언하여 자식 클래스들을 생성하면 관리가 용이할 것이다.



### 다. super

 `this`는 현재 클래스를 가르킨다. 이와 달리 `super`는 상속 받는 부모 클래스를 가르킨다. 따라서 자식 클래스에서 부모 클래스에 접근하고자 할 때, `super`를 통해 접근할 수 있다. 

 만약, 아무것도 상속받지 않는 클래스는 계층 관계가 없을까? 아니다. 모든 클래스의 최상위 클래스는 Object 클래스이다. 즉, extend 키워드를 통해 아무것도 상속 받지 않더라도 최상위 클래스인 Object 클래스를 상속 받는다. Object 클래스의 메소드에는 자주 사용하는 다양한 메소드들이 정의되어 있어, 별도로 import하거나 상속을 할 필요 없이 클래스를 생성하는 것만으로 사용가능하다.

---



#  Ⅱ. 클래스의 다양한 활용

## 1. 내부(inner) 클래스

 클래스 안에 내부 클래스를 정의 함으로써, 내부 클래스 멤버에 쉽게 접근할 수 있는 장점을 가지고 있지만 가독성이 좋지 않고 구현에 용이하지 않아 초기와 달리 현재는 사용하지 않는 추세이다. 객체 안에 객체를 둠으로써 데이터를 공유할 경우 장점인 것 같지만, 로직이 복잡해 질 경우 데이터 손실이 발생할 수 있다.  



## 2. 익명(Anonymous) 클래스

```java
public class C {
    public void go() {
        System.out.println("left side");
    }
}

public class Test {
    public static void main(String args[]) {
        new C() {
            public void go() {
                System.out.println("right side");
            }
        }.go();
    }
}
```

 위의 코드와 같이 익명 클래스는 특정 클래스의 메소드를 오버라이드 한후에 사용하고자 할 경우 사용한다. 또한 오버라이드 후에 오버라이드 된 메소드를 즉시 호출할 수 있다. 익명 클래스는 한번 사용하면 사라지는 특성을 가지고 있으며, 인터페이스나 추상화에 사용 된다.  



## 3. 인터페이스

 인터페이스는 클래스와 달리 객체를 생성할 수 없다. 그럼 인터페이스는 어떤 기능을 할까? 인터페이스는 클래스가 구현해야되는 설계도와 같다고 생각하면 된다. 즉 만약에 인터페이스에서 go, stop과 같은 메소드를 정의해 두었다면 이 인터페이스를 활용하는 클래스는 해당 기능을 구현하여야 한다.  

 이 뿐만 아니라  하나의 클래스에 A, B, C, D라는 인터페이스를 활용할 경우 부모 클래스를 상속 받을 때와 마찬가지로 A - D 인터페이스의 변수를 통해 해당 클래스를 레퍼런스 할 수 있다. 이를 통해 생성하는 클래스는 같더라도 레퍼런스 하는 인터페이스의 기능에 따라 다른 기능을 할 수 있도록 구현할 수 있는 것이다.  

 인터페이스는 `implements`키워드를 통해 사용할 수 있다.  

``` java
public interface Car {
    public void go();
    public void stop();
}

public class SmallCar implements Car {
    @Override
    public void go() {
        System.out.println("move 100M");
    }
    @Override
    public void stop() {
        System.out.println("stop");
    }
}

public class BigCar implements Car {
    @Override
    public void go() {
        System.out.println("move 200M");
    }
    @Override
    public void stop() {
        System.out.println("stop");
    }
}

public class Test {
    public static void main(String args[]) {
        Car small = new SmallCar();
        Car big = new BigCar();
        
        Car vehicle[] = {small, big};
        
        for (int i = 0; i < vehicle.length; i++) {
            vehicle[i].go();
        }
    }
}
```

 위의 코드와 같이 Car라는 인터페이스를 통해, Car 인터페이스를 사용하는 각 클래스를 다룰 수 있다는 것을 알 수 있다.  vechicle이라는 Car 인터페이스 배열에 각각의 클래스를 담을 수 있는 이유는, SmallCar와 BigCar는 다른 클래스 이지만 Car 인터페이스로 레퍼런스하기 때문이다. 이는 부모 클래스를 자식 클래스의 레퍼런스로 사용하는 것과 같은 원리이다.  



## 4. 추상 클래스

 클래스의 공통된 부분을 추상 클래스로 정의하여 이것을 상속하여 사용하는 것을 말한다.  클래스의 기능과 인터페이스의 기능을 같이 한다고 생각하면 이해하기 쉽다.



```java
public abstract class A {
    int num;
    String str;
    
    public void go() {
        this.num++;
    }
    
    public abstract void back();
}
```

 위의 간단한 예시와 같은 추상 클래스가 있을 경우 해당 클래스를 상속 받는 경우 멤버 변수와 메소드를 모두 사용할 수 있다. 하지만 일반 클래스를 상속 받는 것과 달리 추상 클래스의 구현되지 않은 부분을 오버라이드 하여 반드시 구현하여야 하는 차이점이 있다.  

 추상 클래스를 상속하는 클래스에서 동일하게 사용되는 기능들은 똑같이 상속하지만, 클래스에 따라 다르게 사용하고자 하는 기능은 `abstract`라고 명시함으로써 적절하게 오버라이드하여 사용할 수 있는 환경을 제공한다.  



## 5. 람다식

```java
public interface A {
    public void go(int distance);
}

public class Test {
    public static void main(String args[]) {
        A a1 = (int distance) -> { System.out.println(distance); };
        A a2 = distance -> {
            int ret = distance * 2;
            return ret;
        }
        a1.go(10);
        a2.go(20);
    }
}
```

  람다식은 말로 표현하자면 다소 어려울 수 있지만, 코드로 보면 생각보다 쉽게 이해할 수 있다. 익명 클래스 처럼 인터페이스를 레퍼런스로 하여 메소드를 선언을 할 수 있도록 하는 기능을 제공한다.  

  객체 `a1`을 생성하는 것과 같이 하나의 기능만 수행할 경우 한줄로 표현할 수 있으며, 만약 인자가 하나인 경우 `a2`처럼 타입과 괄호를 생략할 수 있다.  이와 같은 기능은 객체를 생성할 필요 없이 간단한 함수의 기능을 사용하고자 할 경우 상당히 유용하게 사용할 수 있는 장점을 가지고 있다.  

 (자바 스크립트 처럼 변수를 선언하면서 함수를 선언하는 것과 유사하다고 생각하면 된다.)  



## 6. String

### 가. String의 특징

 String 객체의 특정 문자열을 초기화 후에 변환하고자 한다면, 다음과 같은 과정이 일어난다. 예를 들어, 추가적으로 문자가 추가된다면 기존의 객체에 새로운 문자를 추가하는 것이 아닌 새로운 공간에 기존의 문자를 복사하고 새로운 문자를 추가해주는 방식을 사용한다. 큰 단점은 아니지만 큰 데이터들을 변환하고자 할 경우 일시적으로 메모리 사용의 효율성이 떨어지게 된다. 하지만, 지속적으로 큰 데이터가 추가되거나 누적되지 않는 이상 큰 차이는 없다.  



### 나. StringBuffer, StringBuilder

 StringBuffer는 Stirng 클래스의 단점을 보완하기 위해 기존의 문자열에 새로운 문자열을 추가할 수 있도록 하는 방식을 제공한다. `StringBuffer s = new StringBuffer("AAAA");`라고 선언하였다면 추가하고자 하는 문자열을 `s.append('B')`를 통해 사용할 수 있다.

 StringBuilder는 StringBuffer 보다 빠른 속도를 제공하지만 동기화를 통해, 데이터의 일관성을 보장하지만 StringBuilder는 오는 즉시 처리하기에 속도가 더 빠르다. 안전성은 StringBuffer가 더 좋다고 하지만 극한의 상황이 아니라면 StringBuilder를 사용하여도 무관하다. StringBuilder 역시 StringBuffer처럼 사용하면 된다.



## 7. Collections

 다양한 상황에 따라 적절한 자료구조를 사용하기 위해 자바에서는 사전에 정의된 다양한 자료구조들을 사용할 수 있는 환경을 제공한다.  



### 가. List

   List는 인터페이스로써, List를 통해 구현되는 것은 Vector, ArrayList, LinkedList가 있다.  단순히 배열을 이용하는 것과 달리 각각의 메소드를 통해 쉽게 자료를 추가, 갱신, 삭제와 같은 작업을 진행할 수 있다.  



### 나. Map

 Map의 인터페이스를 통해 구현되는 HashMap이 있다. HashMap은 해시와 같이 Key-Value 쌍으로 정리가 필요한 데이터를 관리하기에 적합하다.  