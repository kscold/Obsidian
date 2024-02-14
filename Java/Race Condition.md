- 자바에서 애플리케이션 개발을 진행하다 보면 [[멀티스레드(Multi Thread)]]를 활용한 프로그래밍을 경험하게 된다.

- 이로인해 Single Thread로 개발을 진행할 경우에는 발생하지 않았던 몇몇 문제점들이 발생하는데 그 중 하나가 RaceCondtiion이다.

- RaceCondition이란 공유 자원에 대해 여러개의 Thread 또는 Process가 동시에 접근하기 위해 경쟁하는 상태를 의미한다.

## 예시

- 예를 들어 아래와 같은 코드가 있다고 해보자.

```java
int a = 1;
a = a + 1;
```

- 이 때, 프로그램이 동작하는 순서는 다음과 같다.

	1. a를 메모리에 올린다.
	2. a에 1을 더한다.
	3. 결과를 저장한다.

- 만약, 이 프로그램이 Single Thread에서 동작한다면 이 프로그램에 큰 문제는 없다.  
- 그런데, 만약 이 프로그램이 Multi Thread에서 동작하고 있다면 문제가 발생할 수 있다.

- 만약 2개의 [[Thread]]가 존재하고 2개의 [[Thread]] 모두 a에 1을 더하는 동작을 수행한다고 생각해보자.

![](https://velog.velcdn.com/images/squarebird/post/6f8aa616-d85d-4858-a1ef-e0fcbe8f0e21/image.png)

- 위의 표와 같이 동작한다면 우리는 a에 1을 더하는 작업을 두번 하는 코드를 통해 기대하고 있던 3이라는 결과값을 얻을 수 있을것이다. 
- 하지만, 자바에서 여러개의 Thread가 동시에 실행될때는 어떤 Thread가 먼저 실행될지 알 수 없다.

![](https://velog.velcdn.com/images/squarebird/post/4ea7bf4e-908e-4edc-bedd-2e0ab210362d/image.png)

- 그렇기 때문에 위와 같은 결과가 나올 수 있다.

- Thread1에서 a를 메모리에 올리고 1을 더하는 작업이 수행 되었지만, Context Switching이 발생하여 Thread2가 동작하면서 Thread1이 a에 값을 저장하지 않았으므로 다시 1을 가져와서 계산을 한 후 2라는 결과값을 저장한다.
- 그 다음 다시 Context Switching이 발생하면 Thread1은 기존에 a에 1을 저장하는 동작을 수행했으니 다음 동작인 결과를 저장하는 동작을 수행한다.
- 이 때, Thread1이 가지고 있던 값은 기존 a에 1을 더한 값인 2이므로 다시 2가 저장된다.

- 우리는 3이라는 결과값을 기대했지만, Context Switching이 발생하면서 2라는 우리가 기대하지 않은 값을 얻게되었으며, 이와 같은 문제를 Race Condition이라고 한다.

## 해결 방법

- Race Condition을 해결하기 위해서는 임계 구역을 가진 [[Thread]]들이 서로 겹치지 않고 단독으로 실행 즉 상호 배제(Mutual Exclution)되는 것이 보장되어야 한다.

- 그렇다면 이 Race Condition을 해결하기 위한 방법은 어떤것이 있을까?

### 1. Mutex(뮤텍스)

![](https://velog.velcdn.com/images/squarebird/post/7d0072f7-3060-4074-a595-6d90a8894962/image.png)

- 첫 번째 방법은 Mutex이다.
- Mutex를 기반으로 한 상호 배제에서는 Key를 통해 리소스로의 접근을 관리한다.

- Thread가 공유된 리소스에 접근하기 위해서는 Key를 획득해야 하며, Key를 가지고 있지 않은 Thread는 다른 Thread가 리소스를 사용한 뒤 Key를 반납하기를 기다려야 한다.

- 이 때, 이 Key는 [[동기화(Synchronization)]]또는 락(Lock)을 통해 구현한다.

### 2. Semaphore(세마포어)

![](https://velog.velcdn.com/images/squarebird/post/2b634cc9-9b4f-4cd1-9f8a-17020d2582b1/image.png)

- 두 번째 방법은 Semaphore이다.

- Semaphore에서는 Key는 존재하지 않지만, 특정 리소스에 대해 접근할 수 있는 [[Thread]]의 최대 수를 관리하는 값이 존재한다.

- 각 Thread들은 Share Resource영역에 있는 값을 사용하기 전에 먼저 Semaphore의 값을 확인한다.
- 이 때, Semaphore의 값이 0이면 리소스를 사용할 수 없으며 다른 Thread가 리소스를 사용 완료한 뒤 Semaphore의 값이 증가하여 0이 아니게 되면 리소스를 사용할 수 있다.

### Mutex와 Semaphore의 차이점

- 그렇다면 두 방식의 공통점과 차이점은 무엇일까?
- 먼저 공통점은 Thread의 독립적 실행 즉 상호 배제를 달성하기 위한 방법이라는 것이다.

- 차이점은 아래와 같다.
	1. Mutex는 동기화 대상이 오직 1개일 때 사용하며, Semaphore는 동기화 대상이 1개 이상일 때 사용한다.
	2. Mutex는 자원을 소유할 수 있고 책임을 가지며, Semaphore는 자원을 소유할 수 없다.
	3. Mutex는 상태가 0, 1 뿐이므로 Lock을 가질 수 있고 이 Lock을 소유하고 있는 스레드만이 Mutex를 해제할 수 있는 반면, Semaphore는 Semaphore를 소유하지 않은 스레드가 Semaphore를 해제할 수 있다.
	4. Semaphore는 시스템 범위에 걸쳐 있고, 파일 시스템상에 파일로 존재한다. 반면, Mutex는 프로세스의 범위를 가지며 프로세스가 종료될 때 자동으로 Clean up 된다