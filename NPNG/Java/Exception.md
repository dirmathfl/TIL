# 예외 처리

>  에러로 인해 프로그래밍 동작이 멈출 수 있는 상황 등을 예외라고 하며 이를 처리할 수 있는 것을 예외 처리라고 한다. 예외 처리는 크게 `Checked Exception`, `Unchecked Exception` 2가지로 나뉜다.  `Checked Exception`은 반드시 처리하여야 하는 예외로 예외가 처리 되지 않을 경우 컴파일 시 오류가 밸상한다. 이와 달리 `Unchecked Exception`의 경우는 예를 들어 0을 나누고자 하는 경우 오류가 발생하지만 컴파일 시에는 에러를 발생 시키지 않는다. 따라서, 예외 처리와 대표적인 `Checked Exception`인 입출력과 네트워킹에 대해 알아보고자 한다.



# Ⅰ. 예외 처리

## 1. Exception 클래스

Exception의 자식 클래스 중에는 여러 가지가 있다. 예를 들어 자주 쓰이는 `NullPointerException`, `ArrayIndexOutOfBoundExcetpion` 등이 있을 수 있다. 각 하위 Exception들을 모두 기억할 필요는 없다. 이는 상속으로 생기는 특성을 활용하여, 상위 클래스인 Exception을 활용 하여 처리가 가능하기 때문이다.



## 2. try ~ catch

```java
public class Test {
    public static void main(String args[]) {
        int i = 0;
        int j = 0;
		int ret;
        try {
            /* 예외 발생 가능성이 있는 코드 */
            ret = i / j;
        } catch (Exception e) {
            /* 예외가 발생했을 때 처리할 코드 */
            System.out.println(e.getMessage());
        }
    }
}
```

 위의 예제의 경우 `ArithmeticException`이 발생하지만, 최상위 클래스인 `Exception`으로 예외를 처리할 수 있는 것을 알 수 있다.



## 3. throws

```java
public void divisor(int a, int b) throws Exception {
    return a / b;
}
```

 `try ~ catch`와 같이 예외를 직접 처리하는 것이 아닌, 예외 처리가 가능한 곳으로 던져버리는 것이 `throws`이다. 예를 들어 A 함수에서 해당 함수를 호출 하였다면, 이에 대한 예외처리를 하여야 한다. 또한 혼돈하기 쉬운 `throw`가 있는데 이는 강제로 예외를 발생시키는 키워드이다.



# Ⅱ. 입출력

 데이터를 가져오는 것을 입력(Input), 내보내는 것을 출력(Output)이라고 한다. 가장 간단한 것은 Scanner를 통해 받는 입력과 print를 통해 출력하는 값이 있을 수 있다.



## 1. 입출력 클래스

 입출력에 대한 상위 클래스는 `InputStream`과 `OutputStream`이다. 해당 클래스의 자식으로 여러가지 방법으로 입출력을 할 수 있는 방법을 제공한다.

* **FileInputStream / FileOutputStream**
  * Input의 매개변수를 통해 1 byte씩 읽을지, 아니면 설정된 byte 만큼 읽을지를 선택 할 수 있다.
  * Ouput의 매개변수를 통해 byte를 입력하거나, 지점을 선택할 수 있다.
* **DataInputStream / DataOutputStream**
  * byte 단위로 처리하는 것이 아닌, 문자열 단위로 처리 할 수 있도록 한다.
* **BufferedReader / BufferedWriter**
  * buffer를 통해 입출력이 가능하도록 한다.



```java
import java.io.BufferedReader;
import java.io.FileReader;
import java.io.IOException;

public class Test {
	public static void main(String[] args) {
		String fileName = "C:\\test\\a.txt";
		BufferedReader br = null;
		FileReader fr = null;

		try {
			fr = new FileReader(fileName);
			br = new BufferedReader(fr);

			String strLine;

			while ((strLine = br.readLine()) != null) {
				System.out.println(strLine);
			}
		} catch (IOException e) {
			e.printStackTrace();
		} finally {

			try {
				if (br != null) br.close();
				if (fr != null) fr.close();
			} catch (IOException ex) {
				ex.printStackTrace();
			}
		}
	}
}

```

 위의 예시는 파일의 값을 BufferReader로 읽어오는 간략한 예시이다. 코드를 보면 입출력은 `Checked Exception`이기에 예외 처리를 하는 것을 알 수 있다.



# Ⅲ. 네트워킹

## 1. Socket

다른 프로그램과 정보를 주고 받기 위해서는 입출력 클래스를 통해 값을 출력하고, 이를 Socket 클래스를 통해 전달 해준다. Socket은 간단하게 프로그램에서 응답에 대한 요청을 받을 수 있는 창구라고 생각하면 쉽게 이해할 수 있다.



## 2. Socket 클래스

```java
import java.io.IOException;
import java.net.ServerSocket;
import java.net.Socket;

public class TestServer {
	public static void main(String[] args) {
		ServerSocket serverSocket = null;
		Socket socket = null;
		
		try {
			serverSocket = new ServerSocket(9000);
			
			socket = serverSocket.accept();
			System.out.println("socket: " + socket);
			
		} catch (IOException e) {
			e.printStackTrace();
		} finally {
			try {
				if(socket != null) socket.close();
				if(serverSocket != null) serverSocket.close();
			} catch (IOException e) {
				e.printStackTrace();
			}
		}
	}
}

```

```java
import java.io.IOException;
import java.net.Socket;

public class TestClient {
	public static void main(String[] args) {
		Socket socket = null;
		
		try {
			socket = new Socket("localhost", 9000);
			System.out.println("socket: " + socket);
			
		} catch (IOException e) {
			e.printStackTrace();
		} finally {
			try {
				if(socket != null) socket.close();
			} catch (IOException e) {
				e.printStackTrace();
			}
		}
	}
}

```

 위와 같이 Server, Clinet를 Socket 클래스를 통해 구현하여 Server 쪽으로 접속할 수 있는 환경을 만들 수 있다. 네트워킹은 `Checked Exception`이므로 반드시 예외처리를 해주어야 한다. 간단한 예제와 달리 `InputStream`과 `OutputStream`을 활용하면 Server ↔ Client 간의 양방향 통신도 가능하다.

