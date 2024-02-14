- 실행중인 스레드를 잠시 멈추게 하고 싶다면 Thread 클래스의 [[정적 메서드(Static Method)]]인 sleep() 메서드를 사용하면 된다.
- Thread.sleep()메소드를 호출한 스레드는 주어진 시간 동안 일시 정지 상태가 되고 다시 실행 대기 상태로 돌아간다.

## 문법

```java
try {
	Thread.sleep(1000);
} catch(InterruptedException e) {
	e.printStackTrace();
}
```

- 인자에는 얼마 동안 일시 정지 상태로 있을것인지 밀리세컨드 (1/1000) 단위로 시간을 알려주면 된다.
- 위와 같이 1000이라는 값을 주면 스레드는 1초동안 일시 정지 상태가 된다.
- 일시 정지 상태에서 주어진 시간이 되기전에 interrupt() 메소드가 호출되면 InterruptedException이 발생하기 때문에 예외 처리가 필요하다.

## 예시

```java
import java.awt.Toolkit;

public class SleepThread {
	public static void main(String []args){
	    Toolkit toolkit =  Toolkit.getDefaultToolkit();
	    
	    for(int i = 0; i < 10; i++){
            toolkit.beep();
            System.out.println("3초 후 Beep음이 울립니다.");
            
            try {
                Thread.sleep(3000);
            } catch (InterruptedException e) {
                // TODO Auto-generated catch block
                e.printStackTrace();
            }
        }
    }
}
```

- 위의 sleep 예시를 실행시키면, 3초마다 스레드가 정지 상태가 되므로 3초 주기로 beep음이 들리게 된다.

- sleep()에 예외 처리가 되어 있는데, 이는 일시 정지 상태에서 주어진 시간이 되기 전에 interrupt() 메소드가 호출되어 InterruptedException이 발생하기 때문이다.

- 여기서 interrupt()는 일시 정지 상태의 스레드에서 InterruptedException 예외를 발생시켜, 예외 처리 코드(catch)에서 실행 대기 상태로 가거나 종료 상태로 갈 수 있도록 하는 메서드이다.