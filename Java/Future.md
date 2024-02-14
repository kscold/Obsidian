- [[Callable]] [[인터페이스(Interface)]]의 구현체인 작업(Task)은 가용 가능한 [[Thread]]가 없어서 실행이 미뤄질 수 있고, 작업 시간이 오래 걸릴 수도 있다. 

- 그래서 실행 결과를 바로 받지 못하고 미래의 어느 시점에 얻을 수 있는데, 미래에 완료된 Callable의 반환값을 구하기 위해 사용되는 것이 Future이다. 
- 즉, Future는 [[비동기(Asynchronous)]] 작업을 갖고 있어 미래에 실행 결과를 얻도록 도와준다. 

- 이를 위해 [[비동기(Asynchronous)]] 작업의 현재 상태를 확인하고, 기다리며, 결과를 얻는 방법 등을 제공한다.

## 구현

```java
public interface Future<V> {
    boolean cancel(boolean mayInterruptIfRunning);
    boolean isCancelled();
    boolean isDone();
    
    V get() throws InterruptedException, ExecutionException;
    
    V get(long timeout, TimeUnit unit)
		throws InterruptedException, ExecutionException, TimeoutException;
}
```

## 메서드
### get

- 블로킹 방식으로 결과를 가져온다.
- 타임아웃 설정 가능하다.

### isDone, isCancelled

- isDone은 작업의 완료 여부, isCancelled는 작업의 취소 여부를 반환한다.
- 완료 여부를 boolean으로 반환한다.

### cancel

- 작업을 취소시키며, 취소 여부를 boolean으로 반환한다.
- cancle 후에 isDone()는 항상 true를 반환한다.

- cancle의 파라미터로는 boolean 값을 전달할 수 있는데, true를 전달하면 쓰레드를 interrupt 시켜 InterrepctException을 발생시키고 false를 전달하면 진행중인 작업이 끝날때까지 대기한다.

- canel은 작업이 이미 정상적으로 완료되어 취소할 수 없는 경우에는 false를, 그렇지 않으면 true를 반환한다.
- 그 외에도 작업이 이미 취소되었거나 취소가 불가능한 경우에도 false가 반환될 수 있다.

## 예시

- 아래의 코드는 3초가 걸리는 작업의 결과를 얻기 위해 get을 호출하고 있다.
- get은 결과를 기다리는 블로킹 요청이므로 아래의 실행은 적어도 3초가 걸리며, 작업이 끝나고 isDone이 true가 되면 아래의 실행은 종료된다. 

```java
@Test
void future() {
    ExecutorService executorService = Executors.newSingleThreadExecutor();
    
    Callable<String> callable = new Callable<String>() {
        @Override
        public String call() throws InterruptedException {
            Thread.sleep(3000L);
            return "Thread: " + Thread.currentThread().getName();
        }
    };
    
    // It takes 3 seconds by blocking(블로킹에 의해 3초 걸림)
    Future<String> future = executorService.submit(callable);
    
    System.out.println(future.get());
    
    executorService.shutdown();
}
```
