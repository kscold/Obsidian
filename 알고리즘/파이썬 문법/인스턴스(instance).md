인스턴스란 클래스에 의해 만들어진 [[객체(Object)]]를 저장하는 변수 뜻한다.

```python
class Calc:
	def __init__(Calc, a, b): # Calc 부분을 파이썬 내장 함수인 self라고 정의해도 됨
		self.x = a
		self.y = b
	
	def plus(Calc):

        return Calc.x + Calc.y

  

    def minus(Calc):

        return Calc.x - Calc.y

  

    def multiply(Calc):

        return Calc.x * Calc.y

  

    def divide(Calc):

        return Calc.x / Calc.y

  ```

이런한 객체 Calc를 만들었을 때 이를 i = Calc(2,3)이라고 만들었을 때는 i에 Calc이라는 새로운 함수를 i라는 변수 공간에 새로 객체를 할당한 형태가 되므로

이때 객체 i를 Calc의 인스턴스라고 볼 수 있다. 즉 인스턴스는 객체를 복사하고 값을 넣은 객체라고 볼 수 있다. 또한 이때 '__init__'의 함수의 의해 Calc(2, 3)은 self.x = 2 self.y = 3에 대해 대입이 이루어진다.

  

이때 객체인지 확인할 수 있는 함수 isinstance(i, Calc)도 있다. true false로 반환

  

따라서 

  

i.plus()

5

  

i.minus

-1

  

i.multiply()

6

이런식으로 사용할 수 있다 -> i가 현재 Calc(2, 3)으로 설정된 인스턴스이기 때문에