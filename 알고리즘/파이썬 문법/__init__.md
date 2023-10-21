def __init__(self, 변수, 변수, …):

클래스 생성자라고 불리며 인스턴스가 생서되는 시점에 자동으로 생성자인 `__init__(self)` 메서드가 호출됐기 때이다. 따라서 `__init__`은 파이썬에 기본값을 설정하는 명령어로 initialize를 표현한 구문이다. 첫번째 파라미터는 self으로 대체로 지정하여 사용한다.

예를 들어 밑에 와 같은
```
class Calc:
	def __init__(self, a, b):
		self.x = a
		self.y = b
```
로 정의 되어 있을 때 외부 함수 에서 a와 b 값을 받아 __init__ 초기값 지정 함수의 첫번째 파라미터 객체 self에 x를 생성하는데 이 생성한 x에 a를 넣어 초기값을 설정한다는 뜻이다.


```
class Node:  
    def __init__(self, val):  
        self.val = val  
        self.leftChild = None  
        self.rightChild = None
    
```

이런 형식일 때,  Node [[객체(Object)]]를 `__init__`에 의애 Node(val) 바로 이런식으로 사용할 수 있다.