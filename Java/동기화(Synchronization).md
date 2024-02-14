- 자바에서 [[멀티스레드(Multi Thread)]]를 이용하면 여러작업을 동시에 처리할 수 있기 때문에 작업효율이 좋아진다.

- 하지만 하나의 공유자원을 여러 스레드에서 동시에 접근하여 사용하게 되면 때때로 우리가 예상치 못한 결과가 나타나게 된다.

- 스레드([[Thread]]) 동기화는 [[멀티스레드(Multi Thread)]] 환경에서 여러 스레드가 하나의 공유자원에 동시에 접근하지 못하도록 막는것을 말한다. 

- 공유데이터가 사용되어 동기화가 필요한 부분을 임계영역(critical section)이라고 부르며, 자바에서는 이 임계영역에 [[synchronized]] 키워드**를 사용하여 여러 스레드가 동시에 접근하는 것을 금지함으로써 동기화를 할 수 있습니다. 

## synchronized  예시

- [[synchronized ]]키워드는 동기화가 필요한 [[메서드(Method)]]나 코드 블럭앞에 사용하여 동기화 할 수 있다.

- synchronized로 지정된 임계영역은 한 스레드가 이 영역에 접근하여 사용할때 lock이 걸림으로써 다른 스레드가 접근할 수 없게 된다.

- 이후 해당 스레드가 이 임계영역의 코드를 다 실행 후 벗어나게되면 unlock 상태가 되어 그때서야 대기하고 있던 다른 스레드가 이 임계영역에 접근하여 다시 lock을 걸고 사용할 수 있게 된다.

 - lock은 해당 객체당 하나씩 존재하며, **synchronized로 설정된 임계영역은 lock 권한을 얻은 하나의 객체만이 독점적으로 사용**하게됩니다. 

**1) 메소드에 synchronized 설정하기**

메소드 이름 앞에 synchronized 키워드를 사용하면 해당 메소드 전체를 임계영역으로 설정하실수 있습니다. 

```
synchronized void increase() {
	count++;
	System.out.println(count);
}
```

**2) 코드블럭에 synchronized 설정하기**

동기화를 많이 사용하게 되면 효율이 떨어지게 되므로 꼭 필요한 부분에만 블럭을 지정하여 임계영역으로 설정하실 수 있습니다. 예제와 같이 **synchronized(this)로 지정하게 되면 참조변수(this) 객체의 lock을 사용**하게 됩니다. 

```
void increase() {
	synchronized(this) {
		count++;
	}
	System.out.println(count);
}
```

### **3. 동기화 사용예제**

**예제1)** 

```
public class HelloWorld {
	public static void main(String[] args) {
		StringDisplay sd = new StringDisplay();
		MyThread[] mts = new MyThread[5];
		for (int i=0; i<mts.length; i++) {
			mts[i] = new MyThread(sd, Integer.toString(i));	
			mts[i].start();
		}
	}
}

class StringDisplay {
	synchronized void display(String s) {
		for (int i=0; i<5; i++) {
			System.out.print(s);
		}
		System.out.println("");
	}
}

class MyThread extends Thread {
	StringDisplay sd;
	String s = "";
	
	public MyThread(StringDisplay sd, String s) {
		this.sd = sd;
		this.s = s;
	}
	
	@Override
	public void run() {
		sd.display(s);
	}
}

[실행결과]
00000
22222
11111
44444
33333
```

**예제2)** 

```
public class HelloWorld {
	public static void main(String[] args) {
		MyThread[] mts = new MyThread[5];
		for (int i=0; i<mts.length; i++) {
			mts[i] = new MyThread();	
			mts[i].start();
		}
	}
}

class MyThread extends Thread {
	public static int number = 0;
	public static Object lock = new Object(); 
	
	@Override
	public void run() {
		synchronized(lock) {
			for (int i=0; i<5; i++) {
				number = number + 1;;
				System.out.print(i);
			}
			System.out.println(":" + number + "-" + this.getName());
		}
	}
}

[실행결과]
01234:5-Thread-0
01234:10-Thread-2
01234:15-Thread-1
01234:20-Thread-4
01234:25-Thread-3
```

출처: [https://kadosholy.tistory.com/123](https://kadosholy.tistory.com/123) [KADOSHoly:티스토리]