- 콜백 함수(Callback Fuction)는 다른 코드의 인자로서 넘겨주는 실행 가능한 코드를 말한다. 

- 따라서 콜백을 넘겨받는 코드는 이 콜백을 필요에 따라 즉시 실행할 수도 있고, 아니면 나중에 실행할 수도 있다.

- 즉, 콜백함수를 쉽게 말하면 어떠한 행위를 하면 자동으로 실행되는 함수이다.


## 콜백 함수를 쓰는 이유

- 대표적으로 2가지 이유가 있다.

1. 코딩을 하는 모든 사람이 해당 기능을 사용할 때, 똑같은 처리를 하기 위함이다.
2. 비동기처리를 해야할 때 유용하게 사용한다.


## 예시


```java
public void First_Method() { 
	callback_method(); 
} 

public void Callback_Method() { 
	System.print.out("내가 콜백함수다!"); 
}
```

- 위의 코드를 보면 First_Method() [[메서드(Method)]]를 실행했을 때, Callback_Method() [[메서드(Method)]]가 자동으로 실행되는 것을 살펴볼 수 있다.

- 즉, 내가 어떠한 행위를 했을 때First_Method() 호출), 콜백함수(Callback_Method()) 콜(Call)이 되는 것을 살펴볼 수 있다.