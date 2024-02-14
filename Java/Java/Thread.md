- 쓰레드란 프로그램 실행되는 흐름의 가장 작은 단위이다.
- 특히 프로세스 내에서 실행되는 흐름의 단위이다.

- 일반적으로 자바 애플리케이션을 만들어 실행하면 1개의 메인(main) 쓰레드에 의해 프로그램이 실행된다. 

- 하지만 1개의 쓰레드 만으로는 동시에 여러 작업을 할 수 없다. 
- 동시에 여러 작업을 처리하고 싶다면, 별도의 쓰레드를 만들어 실행시켜줘야 하는데, 자바는 멀티 쓰레드 기반으로 동시성 프로그래밍을 지원하기 위한 방법들을 계속해서 발전시켜 왔다.

- 그 중에서 Thread와 [[Runnable]]은 자바 초기부터 멀티 쓰레드를 위해 제공되었던 기술인데, 이 두가지에 대해 먼저 알아보도록 하자.

| Java5 이전 | Runnbale과 Thread |
| ---- | ---- |
| Java5 | Callable과 Future 및 Executor, ExecutorService, Executors |
| Java7 | Fork/Join 및 RecursiveTask |
| Java8 | CompletableFuture |
| Java9 | Flow |
## Thread 클래스

- Thread는 쓰레드 생성을 위해 Java에서 미리 구현해둔 클래스이다. 
- Thread는 기본적으로 다음과 같은 [[메서드(Method)]]들을 제공한다.

### [[Tread.Sleep]]

- 현재 쓰레드 멈출 때 사용한다.
- 자원을 놓아주지는 않고, 제어권을 넘겨주므로 데드락이 발생할 수 있다.

## interupt

- 다른 쓰레드를 깨워서 interruptedException 을 발생시킨다.
- Interupt가 발생한 쓰레드는 예외를 catch하여 다른 작업을 할 수 있다.

### join

- 다른 쓰레드의 작업이 끝날 때 까지 기다리게 한다.
- 쓰레드의 순서를 제어할 때 사용할 수 있다.

## 예시 

- Thread [[클래스(Class)]]로 쓰레드를 구현하려면 이를 [[상속(Inheritance)]]받는 [[클래스(Class)]]를 만들고, 내부에서 run [[메서드(Method)]]를 구현해야 한다. 
- 그리고 Thread의 start 메서드를 호출하면 run 메서드가 실행된다.
- 실행 결과를 보면 main 쓰레드가 아닌 별도의 쓰레드에서 실행됨을 확인할 수 있다.

```java
@Test
void threadStart() {
	Thread thread = new MyThread();
	
    thread.start();
    System.out.println("Hello: " + Thread.currentThread().getName());
}

static class MyThread extends Thread {
    @Override
    public void run() {
        System.out.println("Thread: " + Thread.currentThread().getName());
    }
}

// 출력 결과
// Hello: main
// Thread: Thread-2
```

- 여기서 run을 직접 호출하는 것이 아니라 start를 호출하는 것에 주의해야 한다. 
- 우리는 해당 메소드의 실행을 별도의 쓰레드로 하고 싶은 것인데, run을 직접 호출하는 것은 메인 쓰레드에서 [[객체(Object)]]의 메서드를 호출하는 것에 불과하다.

- 이를 별도의 쓰레드로 실행시키려면 JVM의 도움이 필요하다. 
- 그래서 start를 호출하는 것인데, start 메서드를 자세히 살펴보도록 하자.

```java
public synchronized void start() {
    if (threadStatus != 0)
	    throw new IllegalThreadStateException();
     
    group.add(this);
    
    boolean started = false;
    
    try {
	    start0();
        started = true;
    } finally {
        try {
            if (!started) {
                group.threadStartFailed(this);
            }
        } catch (Throwable ignore) {
        
        }
    }
}
```

- 위의 코드를 보면 알 수 있듯이 start는 크게 다음과 같은 과정으로 진행된다.

	1. 쓰레드가 실행 가능한지 검사한다.
	2. 쓰레드를 쓰레드 그룹에 추가한다.
	3. 쓰레드를 JVM이 실행시킨다.

### 1.쓰레드가 실행 가능한지 검사

- 스레드는 New, [[Runnable]], Waiting, Timed Waiting, Terminated 총 5가지 상태가 있다. 
- start 가장 처음에는 해당 쓰레드가 실행 가능한 상태인지(0인지) 확인한다. 
- 그리고 만약 쓰레드가 New(0) 상태가 아니라면 IllegalThreadStateException 예외를 발생시킨다.

![](https://blog.kakaocdn.net/dn/buLmDm/btrER3drmAo/L8Vw0lq8lB0hkZs01jiijk/img.png)

### 2. 스레드를 스레드 그룹에 추가

- 그 다음 쓰레드 그룹에 해당 쓰레드를 추가시킨다. 
- 여기서 쓰레드 그룹이란 서로 관련있는 쓰레드를 하나의 그룹으로 묶어 다루기 위한 장치인데, 자바에서는 ThreadGroup 클래스를 제공한다.
- 쓰레드 그룹에 해당 쓰레드를 추가하면 쓰레드 그룹에 실행 준비된 쓰레드가 있음을  알려주고, 관련 작업들이 내부적으로 진행된다.

### 3. 쓰레드를 JVM이 실행

- 그리고 start0 메서드를 호출하는데, 이것은 native 메서드로 선언되어 있다.
- 이것은 JVM에 의해 호출되는데, 이것이 내부적으로 run을 호출하는 것이다. 

- 그리고 쓰레드의 상태 역시 Runnable로 바뀌게 된다.
- 그래서 start는 여러 번 호출하는 것이 불가능하고 1번만 가능하다.

```java
private native void start0();
```

- 만약 다음과 같이 run을 직접 호출하면 새롭게 쓰레드가 만들어지지 않고, 메인 쓰레드에 의해 해당 메서드가 실행됨을 확인할 수 있다.
- 또한 여러 번 실행해도 아무런 문제가 없다. 
- 그리고 출력 결과를 보면 main 메소드에 의해 실행됨을 실제로 확인할 수 있다.

```java
@Test
void threadRun() {
    Thread thread = new MyThread();

    thread.run();
    thread.run();
    thread.run();
    System.out.println("Hello: " + Thread.currentThread().getName());
}

// 출력 결과
// Thread: main
// Thread: main
// Thread: main
// Hello: main
```
