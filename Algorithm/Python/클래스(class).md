- 클래스를 이용해 프로그래밍하면 데이터와 데이터를 조작하는 함수를 하나의 묶음으로 관리할 수 있으므로 복잡한 프로그램도 더욱 쉽게 작성할 수 있다.

## 클래스의 정의

```python
class BusinessCard: 
	def set_info(self, name, email, addr):
		self.name = name
		self.email = email 
		self.addr = addr
```

- 이런식으로 class 안에 함수를 정의할 수 있다.

- 이때 항상 클래스 내부에 정의된 함수인 [[메서드(Method)]]의 첫번째 인자는 반드시 [[self]]여야 한다.

- 위 코드에서 메서드 내부를 살펴보면 메서드 인자로 전달된 name, email, addr 값을 self.name, self. email, self.addr이라는 변수에 대입하는 것을 볼 수 있다.
- 파이썬에서 대입은 바인딩을 의미하기 때문에 set_info [[메서드(Method)]]의 동작은  같이 [[메서드(Method)]] 인자인 name, email, addr이라는 변수가 가리키고 있던 값을 self.name, self.email, self.addr이 바인딩하는 것이다.

- BusinessCard 클래스를 새롭게 정의했으므로 새롭게 정의된 클래스로 [[인스턴스(instance)]]를 생성해 한다.
- 붕어빵에 비유해 보면 붕어빵을 굽는 틀을 새롭게 바꿨으므로 새로 붕어빵을 구워보는 것이다.

```python
member1 = BusinessCard() 
# >> member1 <__main__.BusinessCard object at 0x030248F0> 
```

- 만약 [[객체(Object)]]를 내부적으로 들여다 보려면 dir() [[메서드(Method)]]를 사용하면 된다.



