- 동시에 여러 요청을 처리해야 하는 경우에 매번 새로운 [[Thread]]를 만드는 것은 비효율적이다.
- 그래서 쓰레드를 미리 만들어두고 재사용하기 위한 쓰레드 풀(Thread Pool)이 등장하게 되었는데, Executor [[인터페이스(Interface)]]는 쓰레드 풀의 구현을 위한 인터페이스이다.

- 이러한 Executor 인터페이스를 간단히 정리하면 다음과 같다.
	
	- 등록된 작업([[Runnable]])을 실행하기 위한 인터페이스이다.
	- 작업 등록과 작업 실행 중에서 작업 실행만을 책임진다.

- 쓰레드는 크게 작업의 등록과 실행으로 나누어진다.
- 그 중에서도 Executor 인터페이스는 인터페이스 분리 원칙(Interface Segregation Principle)에 맞게 등록된 작업을 실행하는 책임만 갖는다.
- 그래서 전달받은 작업(Runnable)을 실행하는 [[메서드(Method)]]만 가지고 있다.

## 문법

```java
public interface Executor {
	void execute(Runnable command);
}
```

- Executor 인터페이스는 개발자들이 해당 작업의 실행과 쓰레드의 사용 및 스케줄링 등으로부터 벗어날 수 있도록 도와준다. 

## 예시

- 단순히 전달받은 [[Runnable]] 작업을 사용하는 코드를 Executor로 구현하면 다음과 같다.

```java
@Test
void executorRun() {
	final Runnable runnable = () -> System.out.println("Thread: " + Thread.currentThread().getName());
	
    Executor executor = new RunExecutor();
    executor.execute(runnable);
}

static class RunExecutor implements Executor {

	@Override
    public void execute(final Runnable command) {
        command.run();
    }
}
```

- 하지만 위와 같은 코드는 단순히 [[객체(Object)]]의 [[메서드(Method)]]를 호출하는 것이므로, 새로운 쓰레드가 아닌 메인 쓰레드에서 실행이 된다.

- 만약 위의 코드를 새로운 쓰레드에서 실행시키면 Executor의 execute 메서드를 다음과 같이 수정하면 된다. 

```java
@Test
void executorRun() {
    final Runnable runnable = () -> System.out.println("Thread: " + Thread.currentThread().getName());

    Executor executor = new StartExecutor();
    executor.execute(runnable);
}

static class StartExecutor implements Executor {

    @Override
    public void execute(final Runnable command) {
        new Thread(command).start();
    }
}
```
