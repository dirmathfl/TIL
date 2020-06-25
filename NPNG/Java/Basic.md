# 자바 기초 문법

> 모든지 기초가 가장 중요하지 않을까​?​ 아는 내용이지만 다시 한번 리뷰하기 위해 꼼꼼히 작성해 보자.:smile:



## Ⅰ. 자바 실행 구조

> 프로그래밍을 하기 위해 환경설정이 가장 중요하다. 윈도우에서 자바를 사용하기 위해서는 환경 변수 설정이 필요하다. 환경 변수는 리눅스 shell의 환경변수와 비슷하다고 생각하면 이해하기 쉽다. 또한, 자바의 실행 순서에 대해 간략히 알아보고 타 언어와 다른 특징을 알고자 한다.



### 1. 환경 변수 설정

 환경 변수는 시스템 설정에서 설정이 가능하며, **path를 등록하여 변수 이름으로 path를 바로 접근**할 수 있도록 하는 것이다. 환경 변수는 자바 컴파일러나 자바 실행을 다른 디렉토리에서도 가능하게 하기 위해서 등록해두면 별도로 디렉토리를 입력하지 않아도 환경 변수를 통해 접근 할 수 있도록 하는 것이다.

* `JAVA_HOME` : JDK 디렉토리 경로 
  * ex) C:\Program Files\Java\jdk-14.0.1
* `path` : JDK의 하위 bin 경로까지 추가
  * ex) C:\Program Files\Java\jdk-14.0.1\bin



### 2. 자바 실행 순서

 JAVA 소스(.java) → JAVA 컴파일러 →바이트 코드 파일 (.class) → JVM → **기계어 → 실행**

* JVM을 통해 Linking을 하게 된다.
* 호스트 OS에 맞는 JVM을 설치하게 되면, 동일한 JAVA 소스로 실행할 수 있다.
* JVM을 통해 메모리 관리(GC : Garbage Collection)가 이루어 지므로, 개발자가 메모리에 직접 접근할 수 없다.

---



## Ⅱ. 변수와 자료형

> 자바는 다른 언어와 같이 다양한 자료형을 제공한다. 메모리에 담고자하는 데이터에 따라 어떤 변수를 선언하여 초기화 하는 것은 중요하다. 따라서 변수와 자료형을 이해하는 것은 모든 프로그래밍 언어의 출발점이다.



### 1. 변수의 선언과 초기화

```java
public class Test {
    public static void main(String args[]) {
        int i;
        int j = 10;
        /* Below line occurs an error */
        System.out.println(i);
    }
}
```

* `i`와 `j`의 차이는 메모리 공간을 할당하고 `i`는 값을 초기화 하지 않은 것이고, `j`는 값을 초기화 한 것이다.
  * 초기화 : 변수를 메모리에 할당하고 초기 값을 지정한 것을 의미한다.
* 초기화 하지 않는 변수는 사용할 수 없다.



### 2. 자료형

  자료형은 크게 기본 자료형과 객체 자료형으로 나뉘게 된다. 기본 자료형과 객체 자료형의 차이는 다음과 같다. **기본 자료형은 메모리의 할당된 공간에 초기화된 값이나 변경된 값을 할당**하는 것이다. 이와 달리 **객체 자료형은 특정 객체의 주소를 할당**하여 객체에 접근할 수 있다. 

#### 가. 기본 자료형

* 정수형
  * byte : 1 byte
  * char : 2 byte
  * short : 2 byte
  * int : 4 byte
  * long: 8 byte
* 실수형
  * float : 4 byte
  * double : 8 bypte
* 논리형
  * boolean : 1 byte



#### 나. 객체 자료형

 기본 자료형에서 문자를 할당할 수 있는 자료형인 char은 있지만, 문자열을 할당할 수 있는 자료형은 없다. Sting이라는 문자열을 할당할 수 있는 자료형이 있는데 이는 객체 자료형이다. 여기서 드는 의문점은 **String 객체 인데 왜 new method를 사용하여 할당하지 않는가?**라는 의문이 생길 수 있다. String은 많이 사용하기에 new method를 사용하지 않아도 사용할 수 있도록 편의성을 제공하는 것이다.  

 객체 자료형에 할당 된 객체 주소를 레퍼런스라고 하며, 이는 C언어에서 포인터라고 불린다. 객체 주소를 할당하기 위한 레퍼런스의 크기는 4byte로 고정되어 있다.



#### 다. 형 변환

 사용하던 자료형을 변환하고자 할 때, 형 변환이 필요하다. 예를 들어 int로 선언한 변수에 4byte보다 큰 값을 저장하고자 한다면, long으로 변환해 줄 필요가 있다.  

 형 변환은 크게 **묵시적 형 변환**(Implicit Type Casting)과 **명시적 형 변환**(Explicit Type Casting)으로 나뉜다. 둘의 차이는 다음과 같다.



```java
public class Test {
    public static void main(String args[]) {
        /* Implicit Type Casting */
        byte i = 10;
        int j = i;
        
        /* Explicit Type Casting */
		i = (byte)j;
    }
}
```

 크기가 작은 자료형에서 큰 자료형으로 값을 대입하고자 할 경우, 별도의 지정 없이 묵시적 형 변환이 된다. 이와 달리 크기가 큰 자료형에서 작은 자료형으로 값을 대입하고자 할 경우 명시적 형 변환이 필요하다. 당연하게도, 큰 자료형의 데이터를 작은 자료형으로 대입할 경우, 데이터 손실이 발생할 수 있다. 예를 들어, 8byte 공간 만큼의 값을 4byte로 대입하면 어떻게 될지 생각해 본다면 4byte 만큼의 값이 사라진다는 것을 알 수있다.

---



## Ⅲ 배열

 배열은 동일한 자료형을 메모리에 나열하여 한번에 관리할 수 있도록 한다. 즉, 동일한 자료형에 대한 다수의 데이터를 처리하고자 할때 유용하게 사용될 수 있다. 배열의 값에 접근하기 위해서는 인덱스를 사용하여 값에 접근할 수 있다.



### 1. 배열 선언 및 초기화

```java
public class Test {
    public static void main(String args[]) {
        int [] arr1 = new int[5];
        int [] arr2 = {10, 20, 30, 40 ,50};
    }
}
```

 간단한 예시를 보면 `arr1`은 int 형으로 된 5 크기의 배열을 선언만 한 것이고, `arr2`는 int 형으로 된 5 크기의 배열을 선언 후 초기화 한 것이다. 자바는 한번 배열의 크기를 지정하면, 차후에 배열의 크기를 변경하고자 할 경우 변경 할 수 없다.  

 배열을 선언하면 할당 되는 메모리 크기는 `int [] arr1 = new int[5]`로 선언되었다면, 4byte * 5로 20byte의 메모리 공간을 할당한다. 



### 2. 배열 변수의 의미

 자료형에서 언급하였듯이 객체 자료형은 객체의 주소를 할당한다. 또한 주소를 가르키는 것을 레퍼런스라고 한다. 그럼 배열 변수에 할당되는 값은 무엇일까? 다른 언어와 마찬가지로 배열의 시작 주소를 레퍼런스한다.



```java
public class Test {
    public static void main(String args[]) {
        int [] arr1 = {10, 20, 30, 40 ,50};
        int [] arr2 = null;
        int [] arr3 = null;
        
        arr2 = arr1;
        arr3 = Arrays.copyOf(arr1, arr1.length);
    }
}
```

 예를 들어 위와 같은 코드를 실행할 경우 `arr2`와 `arr3`의 주소는 어떻게 될까? arr2는 arr1의 주소를 동일하게 가르키므로, 같은 배열을 사용한다. 이와 달리 arr3는 arr1의 값들을 복사하기 위한 새로운 배열을 할당하고 새로운 배열의 시작주소를 가르킨다.

---



## Ⅳ 조건문과 반복문

### 1. 조건문

#### 가. if 문

```java
public class Test {
    public static void main(String args[]) {
    	int a = 5;
    	int b = 10;
    	
        if (a > b) {
        	System.out.println("a");
        } else if (a == b) {
        	System.out.println("a, b");
        } else {
        	System.out.println("b");
        }
    }
}
```

 `if - else if - else`를 통해 조건에 따라 예시와 같이 처리할 수 있다. 처리 하고자 하는 조건이 2개 혹은 3개 인경우 사용한다.



#### 나. switch 문

```java
public class Test {
    public static void main(String args[]) {
    	int score = 50;
    	
    	switch (score) {
    	case 100:
    	case 90:
    		System.out.println("GOOD");
    		break;
    	case 80:
    		System.out.println("COOL");
    		break;
    	case 70:
    		System.out.println("WOW");
    		break;
    	default:
    		System.out.println("SOSO");
    		break;
    	}
    }
}
```

 여러 조건을 처리 할 필요가 있을 경우 switch 문을 사용하면 된다. if 문과 다른 점은 하나의 조건인 case가 끝이 나면 break를 통해 다음 조건을 실행하지 않도록 하여야 한다는 것이다.



### 2. 반복문

#### 1. for 문

```java
public class Test {
    public static void main(String args[]) {
    	int num = 5;
    	
    	for (int i = 1; i < 10; i++) {
    		System.out.printf("%d * %d = %d\n", num, i, num * i);
    	}
    	
    }
}
```

 지정된 수 만큼 반복문을 실행하고자 할 때 사용된다. 후위 연산자를 통해 증감하는 것이 아닌 감소하는 방식으로 접근 할 수 도 있다.



#### 2. while 문

```java
public class Test {
    public static void main(String args[]) {
    	int num = 5;
    	int i = 1;
    	while (i < 10) {
    		System.out.printf("%d * %d = %d\n", num, i, num * i);
    		i++;
    	}
    }
}
```

 반복문을 언제 멈출지 모르겠을 때,  while을 통해서 반복하며 해당 조건을 만족하면 중지하도록 하는 방식이다. for문과 달리 올바른 조건을 설정하지 않으면, 무한 루프에 빠질 수 있다.



