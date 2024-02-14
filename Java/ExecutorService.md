- ExecutorService는 작업([[Runnable]], Callable) 등록을 위한 [[인터페이스(Interface)]]이다.
- ExecutorService는 [[Executor]]를 상속받아서 작업 등록 뿐만 아니라 실행을 위한 책임도 갖는다. 

- 그래서 [[Thread Pool]]은 기본적으로 ExecutorService 인터페이스를 구현한다. 
- 대표적으로 ThreadPoolExecutor가 ExecutorService의 구현체인데, ThreadPoolExecutor 내부에 있는 블로킹 큐에 작업들을 등록해둔다.

![](https://blog.kakaocdn.net/dn/bcGfch/btrGEhU3Y4W/owqnKjYucjVNZDu4TwEdZ0/img.gif)

- 위와 같이 크기가 2인 쓰레드 풀이 있다고 하자. 
- 각각의 쓰레드는 작업들을 할당받아 처리하는데, 만약 사용가능한 쓰레드가 없다면 작업은 큐에 대기하게 된다. 그러다가 쓰레드가 작업을 끝내면 다음 작업을 할당받게 되는 것이다.

- 이러한 ExecutorService가 제공하는 퍼블릭 메소드들은 다음과 같이 분류 가능하다.
	- 라이프사이클 관리를 위한 기능이다.
	- [[비동기(Asynchronous)]] 작업을 위한 기능이다.

## 라이프사이클 관리를 위한 메서드

- ExecutorService는 [[Executor]]의 상태 확인과 작업 종료 등 라이프사이클 관리를 위한 [[메서드(Method)]]들을 제공하고 있다. 

### shutdown

- 새로운 작업들을 더 이상 받아들이지 않는다.
- 호출 전에 제출된 작업들은 그대로 실행이 끝나고 종료된다.(Graceful Shutdown)

### shutdownNow

- shutdown 기능에 더해 이미 제출된 작업들을 interrupt시킨다.
- 실행을 위해 대기중인 작업 목록([[List]]<[[Runnable]]>)을 반환한다.

### isShutdown

- Executor의 shutdown 여부를 반환한다.

### isTerminated

- shutdown 실행 후 모든 작업의 종료 여부를 반환함

### awaitTermination

- shutdown 실행 후, 지정한 시간 동안 모든 작업이 종료될 때 까지 대기한다.
- 지정한 시간 내에 모든 작업이 종료되었는지 여부를 반환한다.

## 예시

- ExecutorService를 만들어 작업을 실행하면, shutdown이 호출되기 전까지 계속해서 다음 작업을 대기하게 된다.
- 그러므로 작업이 완료되었다면 반드시 shutdown을 명시적으로 호출해주어야 한다. 

```java
@Test
void shutdown() {
    ExecutorService executorService = Executors.newFixedThreadPool(10);
    
    Runnable runnable = () -> System.out.println("Thread: " + Thread.currentThread().getName());
    executorService.execute(runnable);
    
    // shutdown 호출
    executorService.shutdown();
    
    // shutdown 호출 이후에는 새로운 작업들을 받을 수 없음, 에러 발생
    RejectedExecutionException result = assertThrows(RejectedExecutionException.class, () -> executorService.execute(runnable));
    assertThat(result).isInstanceOf(RejectedExecutionException.class);
}
```

- 만약 작업 실행 후에 shtudown을 해주지 않으면 다음과 같이 프로세스가 끝나지 않고, 계속해서 다음 작업을 기다리게 된다. 
- 다음의 메인 메서드를 실행해보면 애플리케이션이 끝나지 않음을 확인할 수 있다.

```java
public static void main(String[] args) {
    ExecutorService executorService = Executors.newFixedThreadPool(10);
    
    Runnable runnable = () -> System.out.println("Thread: " + Thread.currentThread().getName());
    executorService.execute(runnable);

    executorService.shutdown();
}
```

- shutdown과 shutdownNow 시에 중요한 것은, 만약 실행중인 작업들에서 인터럽트 여부에 따른 처리 코드가 없다면 계속 실행된다는 것이다.
- 그러므로 필요하다면 다음과 같이 인터럽트 시에 추가적인 조치를 구현해야 한다.

```java
@Test
void shutdownNow() throws InterruptedException {
    Runnable runnable = () -> {
        System.out.println("Start");
        while (true) {
            if (Thread.currentThread().isInterrupted()) {
                System.out.println("Interrupted");
                break;
            }
        }
        System.out.println("End");
    };

    ExecutorService executorService = Executors.newFixedThreadPool(10);
    executorService.execute(runnable);

    executorService.shutdownNow();
    Thread.sleep(1000L);
}
```

## 비동기 작업을 위한 메서드

- ExecutorService는 [[Runnable]]과 Callbale을 작업으로 사용하기 위한 [[메서드(Method)]]를 제공한다.
- 동시에 여러 작업들을 실행시키는 메소드도 제공하고 있는데, [[비동기(Asynchronous)]] 작업의 진행을 추적할 수 있도록 Future를 반환한다.

- 반환된 Future들은 모두 실행된 것이므로 반환된 isDone은 true이다. 
- 하지만 작업들은 정상적으로 종료되었을 수도 있고, 예외에 의해 종료되었을 수도 있으므로 항상 성공한 것은 아니다. 
- 이러한 ExecutorService가 갖는 비동기 작업을 위한 메서드들을 정리하면 다음과 같다.

## submit

 - 실행할 작업들을 추가하고, 작업의 상태와 결과를 포함하는 Future를 반환한다.
- Future의 get을 호출하면 성공적으로 작업이 완료된 후 결과를 얻을 수 있다.

## invokeAll

- 모든 결과가 나올 때 까지 대기하는 블로킹 방식의 요청한다.
- 동시에 주어진 작업들을 모두 실행하고, 전부 끝나면 각각의 상태와 결과를 갖는 [[List]]<[[Future]]>을 반환한다.

## invokeAny

- 가장 빨리 실행된 결과가 나올 때 까지 대기하는 블로킹 방식의 요청한다.
- 동시에 주어진 작업들을 모두 실행하고, 가장 빨리 완료된 하나의 결과를 Future로 반환받는다.

- ExecutorService의 구현체로는 AbstractExecutorService가 있는데, ExecutorService의 메서드들(submit, invokeAll, invokeAny)에 대한 기본 구현들을 제공한다.
- invokeAll은 최대 쓰레드 풀의 크기만큼 작업을 동시에 실행시킨다. 

- 그러므로 쓰레드가 충분하다면 동시에 실행되는 작업들 중에서 가장 오래 걸리는 작업만큼 시간이 소요된다. 
- 하지만 만약 쓰레드가 부족하다면 대기되는 작업들이 발생하므로 가장 오래 걸리는 작업의 시간에 더해 추가 시간이 필요하다.

## 예시

```java
@Test
void invokeAll() throws InterruptedException, ExecutionException {
    ExecutorService executorService = Executors.newFixedThreadPool(10);
    Instant start = Instant.now();

    Callable<String> hello = () -> {
        Thread.sleep(1000L);
        final String result = "Hello";
        System.out.println("result = " + result);
        return result;
    };

    Callable<String> mang = () -> {
        Thread.sleep(4000L);
        final String result = "Java";
        System.out.println("result = " + result);
        return result;
    };

    Callable<String> kyu = () -> {
        Thread.sleep(2000L);
        final String result = "kyu";
        System.out.println("result = " + result);
        return result;
    };

    List<Future<String>> futures = executorService.invokeAll(Arrays.asList(hello, mang, kyu));
    for(Future<String> f : futures) {
        System.out.println(f.get());
    }

    System.out.println("time = " + Duration.between(start, Instant.now()).getSeconds());
    executorService.shutdown();
}
```

- invokeAny는 가장 빨리 끝난 작업 결과만을 구하므로, 동시에 실행한 작업들 중에서 가장 짧게 걸리는 작업만큼 시간이 걸린다.
- 또한 가장 빠르게 처리된 작업 외의 나머지 작업들은 완료되지 않았으므로 cancel 처리되며, 작업이 진행되는 동안 작업들이 수정되면 결과가 정의되지 않는다.

```java
@Test
void invokeAny() throws InterruptedException, ExecutionException {
    ExecutorService executorService = Executors.newFixedThreadPool(10);
    Instant start = Instant.now();

    Callable<String> hello = () -> {
        Thread.sleep(1000L);
        final String result = "Hello";
        System.out.println("result = " + result);
        return result;
    };

    Callable<String> mang = () -> {
        Thread.sleep(4000L);
        final String result = "Java";
        System.out.println("result = " + result);
        return result;
    };

    Callable<String> kyu = () -> {
        Thread.sleep(2000L);
        final String result = "kyu";
        System.out.println("result = " + result);
        return result;
    };

    String result = executorService.invokeAny(Arrays.asList(hello, mang, kyu));
    System.out.println("result = " + result + " time = " + Duration.between(start, Instant.now()).getSeconds());

    executorService.shutdown();
}
```