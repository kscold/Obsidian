- Runnbale 인터페이스는 1개의 [[메서드(Method)]] 만을 갖는 [[함수형 인터페이스]]이다. 
- 그렇기 때문에 [[람다(lambda)]]로도 사용 가능하다.

```java
@FunctionalInterface
public interface Runnable {

    public abstract void run();
    
}
```

- 이것은 [[Thread]]를 구현하기 위한 템플릿에 해당하는데, 해당 인터페이스의 구현체를 만들고 [[Thread]] [[객체(Object)]] 생성 시에 넘겨주면 실행 가능하다. 
- 앞서 살펴본 Thread 클래스는 반드시 run 메서드를 구현해야 했는데, Thread 클래스가 Runnable를 구현하고 있기 때문이다.

```java
public class Thread implements Runnable {
    ...
}
```

- 기존에 Thread로 작성되었던 코드를 Runnable로 변경하면 다음과 같다.
- 마찬가지로 별도의 쓰레드에서 실행됨을 확인할 수 있다.

```java
@Test
void runnable() {
    Runnable runnable = new Runnable() {
        @Override
        public void run() {
            System.out.println("Thread: " + Thread.currentThread().getName());
        }
    };

    Thread thread = new Thread(runnable);
    thread.start();
    System.out.println("Hello: " + Thread.currentThread().getName());
}

// 출력 결과
// Hello: main
// Thread: Thread-1
```

- Thread.currentThread().getName() 코드로 현재 동작중인 쓰레드의 번호를 알 수 있다.
## Thread와 Runnable 비교

- Runnable은 익명 객체 및 람다로 사용할 수 있지만, Thread는 별도의 클래스를 만들어야 한다는 점에서 번거롭다. 

- 또한 자바에서는 [[다중 상속]]이 불가능하므로 Thread 클래스를 상속받으면 다른 클래스를 상속받을 수 없어서 좋지 않다.

- 또한 Thread 클래스를 상속받으면 Thread 클래스에 구현된 코드들에 의해 더 많은 자원(메모리와 시간 등)을 필요로 하므로 Runnable이 주로 사용된다. 

- 물론 Thread 관련 기능의 확장이 필요한 경우에는 Thread 클래스를 상속받아 구현해야 할 때도 있다.
- 하지만 거의 대부분의 경우에는  Runnable 인터페이스를 사용하면 해결 가능하다.

|  | Runnable | Thread |
| ---- | ---- | ---- |
| 람다 가능 | O | X |
| 상속 필요 | X | O |
| 자원 사용량 | 적음 | 많음 |
## Thread와 Runnable의 단점 및 한계

- 하지만 위에 코드를 통해 보았듯이 Thread와 Runnable을 직접 사용하는 방식은 다음과 같은 한계점이 있다. 

	- 지나치게 저수준의 API(쓰레드의 생성)에 의존한다.
	- 값의 반환이 불가능하다.
	- 매번 쓰레드 생성과 종료하는 오버헤드가 발생한다.
	- 쓰레드들의 관리가 어렵다.

- 먼저 Thread와 Runnable은 쓰레드를 생성하는데 너무 저수준의 API들을 필요로 한다. 
- 쓰레드를 어떻게 만드는지는 애플리케이션을 만드는 개발자의 관심사와는 거리가 멀다. 
- 그리고 쓰레드의 작업이 끝난 후의 결과 값을 반환받는 것도 불가능하다.
- 또한 쓰레드를 사용하려면 항상 새롭게 쓰레드를 생성하고 종료해야 하는데, 이는 비용이 많이 드는 작업들며 직접 쓰레드를 만드는 만큼 쓰레드의 관리 역시 어렵다.

- 따라서 자바5 부터는 [[Thread]]를 더 발전시켜 [[Executor]]와 [[ExecutorService]] 등을 사용한다.