- 멀티스레드 환경에서는 여러 스레드가 변경 가능한 공유 데이터를 동시에 수정하려 할 때 레이스 컨디션이 발생한다. 

- 자바에서는 이러한 [[Race Condition]]을 회피할 수 있도록 synchronized 기능을 제공한다.
- synchronized 키워드를 붙이면 해당 블록에는 오직 하나의 [[Thread]]만 접근 가능하게 된다.

## 예시


```java
public class SynchronizedMethods {

    private int sum = 0;

    public void calculate() {
        setSum(getSum() + 1);
    }

    // standard setters and getters
}
```

```java
@Test
public void givenMultiThread_whenNonSyncMethod() {
    ExecutorService service = Executors.newFixedThreadPool(3);
    SynchronizedMethods summation = new SynchronizedMethods();

    IntStream.range(0, 1000)
      .forEach(count -> service.submit(summation::calculate));
    service.awaitTermination(1000, TimeUnit.MILLISECONDS);

    assertEquals(1000, summation.getSum());
}
```

- 기대하는 결과는 1000이지만 3개의 스레드 풀에서 스레드가 동시에 돌아가며 작업을 하기에 원하는 결과값을 얻을 수 없게 된다.  
- 이러한 레이스 컨디션을 피하기 위해선 synchronized 키워드를 사용하여 스레드 세이프하게 코드를 짜야 한다.

```java
public synchronized void synchronisedCalculate() {
    setSum(getSum() + 1);
}
```

- 위 calculate() 메서드에 synchronized 키워드를 붙인다면 우리가 원하는 결과값을 얻어낼 수 있다.

## synchronized 키워드 종류

- synchronized 키워드는 [[인스턴스 메서드(instance method)]], [[정적 메서드(Static Method)]], 그리고 코드 블록에 붙여서 사용할 수 있다. 

- synchronized 블록을 사용할 때 자바에서는 내부적으로 모니터 락 혹은 고유 락으로 불리는 모니터를 사용하여 동기화를 제공한다. 
- 이 모니터는 객체에 바운드되어있다. 

- 그러므로 synchronized 블록에 들어가려면 객체의 모니터를 획득해야 하며 같은 객체의 synchronized 블록은 동시에 하나의 스레드만 접근 가능하게 된다.

- 다시 말해 synchronized의 범위는 [[객체(Object)]] 단위인 것이다.

#### Synchronized Instance Methods

위의 예제 코드와 같이 인스턴스 메서드에 synchronized를 붙일 수 있다. 멀티 스레드 환경에서 해당 synchronized 메서드에 하나의 스레드만 접근 가능하게 된다. 이때의 대기하는 스레드들의 상태는 BLOCKED이다. ([자바의 Thread 참조](https://velog.io/@destiny1616/%EC%9E%90%EB%B0%94%EC%9D%98-Thread))  
위에서 말했듯이 synchronized 메서드는 객체의 모니터를 획득하고 다른 스레드들의 접근을 막는다. 이 때, 접근을 막는다는 것은 해당 메서드로의 접근을 막는다는거지 객체 자체에 접근 불가한게 아니다. 그러므로 아래와 같이 synchronized 키워드가 붙지 않은 normalCalculate() 메서드에는 모든 스레드가 동시에 접근 가능하다.

```java
public class SynchronizedMethods {

    private int sum = 0;

    public synchronized void synchronisedCalculate() {
        setSum(getSum() + 1);
    }

    public void normalCalculate() {
    	System.out.println("normal calc");
    }

    // standard setters and getters
}
```

마찬가지로 아래 코드 역시 테스트를 통과한다. 각 객체에 락을 거는 것이기에 서로 영향을 받지 않는 것이다.

```java
@Test
public void givenMultiThread_whenNonSyncMethod() {
    ExecutorService service = Executors.newFixedThreadPool(3);

    SynchronizedMethods summation1 = new SynchronizedMethods();
    SynchronizedMethods summation2 = new SynchronizedMethods();

    IntStream.range(0, 1000)
      .forEach(count -> service.submit(summation1::calculate));

    IntStream.range(0, 1000)
      .forEach(count -> service.submit(summation2::calculate));
    service.awaitTermination(1000, TimeUnit.MILLISECONDS);

    assertEquals(1000, summation1.getSum());
    assertEquals(1000, summation2.getSum());
}
```

### Synchronized Static Methods

인스턴스 메서드가 아닌 정적 메서드에도 synchronized 키워드를 걸 수 있다.

(밸덩 예제코드)

```java
public class SynchronizedMethods {
    private static int staticSum = 0;

    public static synchronized void syncStaticCalculate() {
	     	staticSum = staticSum + 1;
	 	}
}
```

```java
@Test
public void givenMultiThread_whenStaticSyncMethod() {
    ExecutorService service = Executors.newCachedThreadPool();

    IntStream.range(0, 1000)
      .forEach(count -> 
        service.submit(BaeldungSynchronizedMethods::syncStaticCalculate));
    service.awaitTermination(100, TimeUnit.MILLISECONDS);

    assertEquals(1000, BaeldungSynchronizedMethods.staticSum);
}
```

여기서 주의할 점은 정적 메서드 역시 객체에 락을 거는 것이며 이 때의 객체는 Class object, 즉 클래스 객체 바로 그 자체이다.

(밸덩 예제코드 변형)

```java
public class SynchronizedMethods {
	private static int staticSum = 0;
    private int sum = 0;

    public static synchronized void syncStaticCalculate() {
    	System.out.println("static synchronized method")
    	Thread.sleep(10);
	    staticSum = staticSum + 1;
	 }

    public synchronized void calculate() {
    	System.out.println("instance synchronized method")
    	Thread.sleep(10);
        setSum(getSum() + 1);
    }

    // standard setters and getters
}
```

```java
@Test
public void givenMultiThread_whenStaticSyncMethod() {
    SynchronizedMethods summation = new SynchronizedMethods();

    new Thread(() -> {
			for (int i = 0; i < 1000; i++) {
				SynchronizedMethods.syncStaticCalculate();
			}
		}).start();

		new Thread(() -> {
			for (int i = 0; i < 1000; i++) {
				summation.calculate();
			}
		}).start();

    assertEquals(1000, SynchronizedMethods.staticSum);
    assertEquals(1000, summation.getSum());
}
```

위의 테스트는 통과한다. calculate() 메서드는 summation 객체에 락을 거는 것이고 syncStaticCalculate() 메서드는 SynchronizedMethods 클래스 객체에 락을 거는 것이기 때문이다.

이렇게 서로 다른 락을 물고 있는 이유로 인해 콘솔에 찍히는 메시지도 순서가 지켜지지 않고 막무가내로 찍힐 것이다(같은 객체를 물고 있다면 한 스레드가 계산을 다하고, 콘솔에 메시지 찍는 작업까지 다 끝나야 다른 스레드가 접근 가능하기 때문). 다시 말해, 정적 synchronized 메서드와 인스턴스 synchronized 메서드를 혼용해서 사용하면 동기화 이슈가 발생할 수 있다는 뜻이다. 같은 객체에 락을 걸것이라 착각하지 말자.

### Synchronized Blocks Within Methods

그런데 메서드 단위로 synchronized를 거는 것은 비효율적일 수도 있다. race condition이 발생하는 부분은 특정 코드일 수 있는데 메서드 전체에 락이 걸려버리기 때문이다.

```java
public class Calc {
    private int sum = 0;

    public synchronized void calculate() {
        sum += 1;
        System.out.println(num);
    }
}
```

예를 들어 이런 경우이다. 문제가 되는 부분은 `sum += 1;` 인거니 저 부분만 동기화처리가 되면 되는데 콘솔에 출력하는 부분까지 synchronized가 걸리는 것이다( System.out.println 자체에 synchronized가 걸려있는건 별개로 치자. println 관련 내용은 [여기에서](https://velog.io/@destiny1616/System.out.println-%EC%82%AC%EC%9A%A9%EC%9D%84-%EC%9E%90%EC%A0%9C%ED%95%B4%EC%95%BC-%ED%95%98%EB%8A%94-%EC%9D%B4%EC%9C%A0) 확인).

이러한 이슈와 관련된 유명한 케이스 중 하나가 바로 HashTable과 같은 레거시 컬렉션 프레임워크의 put 메서드 이다.

```java
public synchronized V put(K key, V value) {
    ...
}
```

이는 HashTable put 메서드 시그니쳐이다. 메서드 전체에 synchronized가 걸려있기 때문에 put이 실행될 때 해시 충돌이 발생하게 되면 해시 충돌 해결하기 위한 코드가 성능 이슈를 발생시킨다.  
이를 보완하기 위해 ConcurrentHashMap에서는 put 메서드 자체에 synchronized를 걸지 않고 해시충돌 발생 처리 부분에만 synchronized를 건다.

특정 부분에만 synchronized를 거는 것이 바로 synchronized 블록이다.

```java
public class Calc {
    private int sum = 0;

    public void increase() {
        synchronized(this) {
            sum += 1;    
        }
        System.out.println(num);
    }
}
```

위의 synchronized 블록에서 this는 Calc 객체를 말한다.

```java
public class Calc {
    private int sum = 0;

    public void increase() {
        synchronized(this) {
            sum += 1;    
        }
        System.out.println(num);
    }

    public synchronized void decrease() {
        sum -= 1;
        System.out.println(num);
    }
}
```

즉 위와 같이 synchronized 메서드가 존재한다면 스레드가 decrease() 메서드를 수행하는 동안에는 increase()의 synchronized 블록에 접근 못한다는 뜻이다.

```java
public class Calc {
    private int sum = 0;
    private Object lock = new Object();

    public void increase() {
        synchronized(lock) {
            sum += 1;    
        }
        System.out.println(num);
    }

    public synchronized void decrease() {
        sum -= 1;
        System.out.println(num);
    }
}
```

위와 같이 임의의 락 객체를 하나 만들어 synchronized 블록에서 사용하게 될 경우, 다른 스레드가 해당 블록에 접근 중일 때 동시에 decrease() 메서드에도 접근이 가능하다.

### 메모리 가시성과 happens-before

synchronized 키워드를 사용하게 된다면 volitile처럼 메모리 가시성을 확보할 수 있다. 그리고 이 가시성 확보를 보장하기 위한 happens-before 정책 역시 존재한다.  
(volitile의 메모리 가시성과 happens-before에 대한 글은 [여기](https://velog.io/@destiny1616/%EB%B3%BC%EB%9D%BC%ED%8B%80) 참조)

#### 가시성

- 스레드가 synchronized 블록에 진입할 때 모든 변수들은 메인메모리로부터 읽혀지게 된다.
- 스레드가 synchronized 블록을 빠져나갈 때 모든 변수들은 메인메모리에 쓰여지게 된다.

#### Java Synchronized Block Beginning Happens Before Guarantee

synchronized 블록 진입시에 가시성을 보장하기 위해서 리오더링에 대한 제약이 필요하다. 즉, synchronized 블록 후에 나오는 변수의 읽기 작업은 synchronized 블록 전으로 리오더링 되어서는 안된다.

```java
public void get(Values v) {
    synchronized(this) {
        v.valC = this.valC;
    }
    v.valB = this.valB;
    v.valA = this.valA;
}
```

위의 코드상에서 this.valB와 this.valA를 읽는 작업은 synchronized 블록 앞으로 리오더링 되지 않는다.

#### Java Synchronized Block End Happens Before Guarantee

마찬가지로 synchronized 블록을 빠져나갈때의 가시성을 위해서도 리오더링 제약이 걸린다. synchronized 블록 전에 나오는 쓰기 작업은 synchronized 블록 후로 리오더링 되어서는 안된다.

```java
public void set(Values v) {
    this.valA = v.valA;
    this.valB = v.valB;
    synchronized(this) {
        this.valC = v.valC;
    }
}
```

위 코드에서 this.valA, this.valB에 대한 쓰기 작업은 synchronized 블록 후로 리오더링 되지 않는다.

### Reentrancy

스레드는 획득한 락에 대해 재진입할 수 있다. 즉 현재 스레드가 같은 synchronized 락에 대해, 해당 락을 쥐고 있는 한 몇 번이고 재획득 할 수 있다는 뜻이다.

```java
public class Reentrancy {
    public synchronized void syncMethod1() {
        System.out.println("method 1");

        syncMethod2();
    }

    public synchronized void syncMethod2() {
        System.out.println("method 2");
    }
}
```

syncMethod1을 수행하고 있는 스레드가 해당 synchronized 메서드 진입시 락을 획득하였으므로 syncMethod2()를 호출 가능한 것이다.  
만약 Reentrancy 특성이 없었더라면 해당 스레드는 syncMethod1에서 syncMethod2를 호출한 후 락 획득을 위해 대기할테고 자기 자신이 락을 소유하고 있으므로 결국 데드락이 발생하게 될 것이다.

### 멀티스레드 환경에서의 싱글톤

아래는 기본적인 싱글톤 생성 방법이다.

```java
public class BasicSingleton {

    private static BasicSingleton instance;

    public static BasicSingleton getInstance() {
        if (instance == null) {
            instance = new BasicSingleton();
        }
        return instance;
    }
}
```

위 방식의 싱글톤 생성은 멀티스레드 환경에서 getInstance() 메서드에 여러 스레드가 동시에 접근할 경우 객체가 2개 이상 생길 가능성이 있다. 이러한 동기화 이슈를 해결해야 한다. 간단한 방법은 getInstance() 메서드를 synchronized하게 선언하는 것이다.

```java
public class BasicSingleton {

    private static BasicSingleton instance;

    public static synchronized BasicSingleton getInstance() {
        if (instance == null) {
            instance = new BasicSingleton();
        }
        return instance;
    }
}
```

이제 위의 싱글톤 생성 방식은 스레드 세이프 해졌으나, 성능 이슈가 발생하게 된다. 싱글톤 객체를 얻기 위해 모든 스레드들은 반드시 락을 획득해야만하고 락을 획득한 스레드 하나만 접근 가능하기 때문이다.  
또한 synchronized 메서드 안에 객체가 이미 생성됐는지 아닌지를 확인하는 로직이 포함되어 있기에 최초 싱글톤 객체가 생성된 이후에도 불필요하게 해당 로직을 거쳐야 한다. 다시 말해 락 획득을 대기하는 스레드들에 불필요한 대기시간이 추가된다는 것이다.

이러한 성능 이슈를 해결하기 위한 한가지 방법으로 Double checked locking 패턴이 소개되었다.

```java
public class DclSingleton {
    private static volatile DclSingleton instance;
    public static DclSingleton getInstance() {
        if (instance == null) {
            synchronized (DclSingleton .class) {
                if (instance == null) {
                    instance = new DclSingleton();
                }
            }
        }
        return instance;
    }

}
```

메서드에 synchronized을 거는 것이 아닌 synchronized 블록을 활용하는 방법이다. 객체를 생성해야 되는지를 synchronized 전에 한 번 체크를 하고, 만약 생성해야 된다면 synchronized 블록 내에서 한번더 더블체크 후 객체를 생성하는 방식이다. 또한 객체 가시성이 확보되어야 하므로 반드시 volatile 키워드를 통해 메인메모리로부터 정보를 읽어와야 한다.  
객체를 생성할 필요가 없다면 굳이 synchronized 블록에 진입할 필요없이 바로 싱글톤 객체를 반환한다.

하지만 이 역시 이슈가 있다. 한 스레드가 접근하여 객체 생성을 하고자 메모리 공간을 할당한 시점에 다른 스레드가 접근할 경우, 아직 객체 생성이 완전히 끝나지 않았음에도 뒤따라온 스레드는 객체가 생성됐다 판단하여 오작동을 일으킬 가능성이 있는 것이다.

이렇게 개발자가 직접 코드로 처리하는 방법이 아닌 동기화 작업을 JVM에 위임하는 방식이 존재한다. (주로 이러한 방식이 사용된다 한다. 추가 조사 필요.)

#### Early Initialization

```java
public class EarlyInitSingleton {
    private static final EarlyInitSingleton INSTANCE = new EarlyInitSingleton();

    public static EarlyInitSingleton getInstance() {
        return INSTANCE;
    }
}
```

가장 쉬운 방법은 static 하게 객체를 생성하는 것이다. 자바에서 정적 필드 초기화는 JVM에 의해 동기화가 보장되므로 이러한 이점을 사용해 싱글톤 패턴을 구현할 수 있게 된다.

#### Initialization on Demand

```java
public class InitOnDemandSingleton {

    private static class InstanceHolder {
        private static final InitOnDemandSingleton INSTANCE = new InitOnDemandSingleton();
    }

    public static InitOnDemandSingleton getInstance() {
        return InstanceHolder.INSTANCE;
    }
}
```

getInstance() 메서드가 호출되는 시점에 InstanceHolder 클래스가 로딩되며 초기화가 진행된다. 지연 초기화라 할 수 있다. 위에서 말했듯이 자바에서 정적 필드 초기화의 동기화가 보장된다.

이렇듯 volitile이나 synchronized 키워드가 없어도 JVM에 의해 동기화가 보장되므로 성능이 좋다.

### synchronized 주의점

1. synchronized 블록 내 객체 수정 주의

```java
public class TestObject {
    private int value;

    TestObject(int value) {
        this.value = value;
    }

    // getter, setter
}

public class Calc {
    private int sum = 0;
    private TestObject obj = new TestObject(100);

    public void increase() {
        synchronized(obj) {
            sum += 1;    

            ojb = new TestObject(200);
        }
        System.out.println(num);
    }
}
```

위의 코드는 제대로 작동하지 않는다. synchronized 블록에서 사용하는 락 객체를 해당 블록 내에서 수정하기 때문이다. 새로 들어온 스레드는 첫 번째 스레드가 획득한 락 객체가 아닌 다른 객체를 물고 들어올 수 있게 되는 것이다. 그러므로 synchronized 블록 내에서 락으로 쓰고 있는 객체의 수정이 있어선 안될 것이다.

2. 성능 관련 주의

```java
public class Test {
    public synchronized void testA() {
        System.out.println("testA");
    }

    public synchronized void testB() {
        System.out.println("testB");
    }

    public synchronized void testC() {
        System.out.println("testC");
    }
}
```

synchronized 키워드를 걸면 성능에 유리하지 않다는 것은 알고 있을 것이다. 위와 같은 코드는 특히나 더 문제다. 위에서 반복하여 말해왔듯이 synchronized 키워드를 붙이면 해당 클래스의 객체에 락을 건다. 즉 한 스레드가 testA() 메서드에서 작업하고 있을 때, Test의 객체에 락을 건 상태일 것이므로 testB()나 testC()에 접근하려는 다른 스레드들은 testA() 실행을 위해 락을 쥐고 있는 스레드로 인해 대기해야만 한다.

3. Synchronized Static Methods와 Synchronized Instance Methods의 혼용

Synchronized Static Methods 섹션에서 이미 말했듯이 정적 synchronized 메서드는 클래스 자체에 락을 걸고 인스턴스 메서드는 객체에 걸기 때문에 혼용해서 사용한다면 서로 다른 객체 락을 물고 있다는 것을 인지하고 있어야 한다. 같은 락 객체를 물고 있다고 착각하고 코드를 짜게 된다면 동기화 이슈에 빠지게 될 것이다.