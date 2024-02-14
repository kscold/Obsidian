- ServerSocket, Socket

- ServerSocket은 java에서 서버 프로그램을 개발할 때 쓰이는 클래스이다. 
- 해당 클래스를 이용해서 서버를 개발 하는 방법에 대해 알아보겠다. 
- Socket 클래스는 client에서 서버로 접속하거나 Server에서 accept 하는데 필요한 클래스다. 이 설명을 이해 하려면 TCP/IP 접속 송수신 과정이해가 선행 되어야 한다.

  
- ServerSocket, Socket 클래스를 이용한 서버-클라이언트 프로그램을 이해하려면 다음과 같이 TCP/IP 송수신 과정을 이해 해야한다.

## 메서드

- ServerSocket: 클라이언트의 요청을 받기 위한 준비를 한다.
- accept: 클라이언트의 요청을 받아 들인다.(Socket은 서버에 접속 요청을 한다.)

- BufferedReader: 클라이언트가 보낸 데이터를 출력 한다.(BufferedWriter은 서버에 메시지를 보낸다.)

- BufferedWriter: 클라이언트에 메시지를 보낸다.(BufferedReader: 서버가 보낸 메시지를 출력한다.)
- socket.close(): 종료 한다. 

```java
// 서버 쪽 소스코드
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.net.ServerSocket;
import java.net.Socket;

  

public class Server {
	public static void main(String[] args ) {
		try {
			// 서버 생성
			ServerSocket serverSocket = new ServerSocket(9000);
			
			//client 접속 accept
			Socket socket = serverSocket.accept();
			
			//client가 보낸 데이터 출력
			BufferedReader bufReader = new BufferedReader( new InputStreamReader( socket.getInputStream()));
			
			String message = bufReader.readLine();
			System.out.println("Message : " + message );
			
			//client에 데이터 전송
			BufferedWriter bufWriter = new BufferedWriter( new OutputStreamWriter( socket.getOutputStream()));
			
			bufWriter.write("hello world");
			bufWriter.newLine();
			bufWriter.flush();
			
			socket.close();
			serverSocket.close();
			
			bufReader.close();
			bufWriter.close();
		}
		
		catch(Exception e) {
			e.printStackTrace();
		}
	}
}
```

- ServerSocket serverSocket = new ServerSocket(9000); 코드로 9000번 port로 설정해 서버를 생성한다.
- Socket socket = serverSocket.accept(); 클라이언트 요청을 받아 들인다.

클라이언트

```java
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.net.Socket;

public class Client {
	public static void main(String[] args ) {
	
		try {
			//서버 접속
			Socket socket = new Socket("127.0.0.1", 9000);
			
			//Server에 보낼 데이터
			BufferedWriter bufWriter = new BufferedWriter( new OutputStreamWriter( socket.getOutputStream()));
			
			bufWriter.write("hello world");
			bufWriter.newLine();
			bufWriter.flush();
			
			//Server가 보낸 데이터 출
			
			BufferedReader bufReader = new BufferedReader( new InputStreamReader( socket.getInputStream()));
			
			String message = bufReader.readLine();
			System.out.println("Message : " + message );
			
			socket.close();
			bufReader.close();
			bufWriter.close();
		}
		catch(Exception e ){
			e.printStackTrace();
		}
	}
}
```

- Socket socket = new Socket("127.0.0.1", 9000); 코드로 127.0.0.1 (localhost) 9000번 서버에 접속한다.

- 소켓에서 데이터를 읽을 때 예시 기준으로 서버인 경우 클라이언트의 요청을 받는다.
- 반대로 클라이언트는 서버의 응답을 받는다.

- BufferedReader bufReader = new BufferedReader(new InputStreamReader(socket.getInputStream()))); 소켓에서 데이터를 쓸 때 예시 기준으로 서버인 경우 클라이언트에 응답을 한다.
- 반대로 클라이언트는 서버의 요청을 한다.

- BufferedWriter bufWriter = new BufferedWriter(new OutputStreamWriter(socket.getOutputStream())))