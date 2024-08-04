- 일급객체(First-class Object)란 다른 [[객체(Object)]]들에 일반적으로 적용 가능한 연산을 모두 지원하는 [[객체(Object)]]를 가리킨다. 

 - [[변수(Variable)]]에 할당(assignment)할 수 있다.
 - 다른 함수를 인자(argument)로 전달 받는다.
 - 다른 함수의 결과로서 리턴될 수 있다.

- 위에 대한 조건으로 인해 알 수 있는 것은 [[함수(Function)]]를 데이터(`string`, `number`, `boolean`, `array`, `object`) 다루 듯이 다룰 수 있다는 점이다.

## 다른 언어와의 [[객체(Object)]]의 정의 구별

- 자바스크립트에서 함수는 indeed 객체이다. 이것은 JavaScript가 "일급 함수"를 지원하는 함수형 프로그래밍 언어로 분류되는 이유 중 하나이다.

1. C 및 C++: 이러한 언어에서 함수는 주로 프로시저(procedure) 또는 메서드(method)로 정의됩니다. 이러한 언어에서는 함수가 객체로 간주되지 않으며 일반적으로 다른 데이터 유형과 구분된다.

2. Python: Python에서도 함수는 객체입니다. 함수는 변수에 할당하거나 다른 함수의 인수로 전달할 수 있습니다. 이러한 특성은 파이썬에서 함수형 프로그래밍 스타일을 지원하게 해준다.

3. Java: Java에서는 함수가 객체로 정의되지 않습니다. 함수는 클래스 내의 메서드로 정의되며 객체로 간주됩니다. 함수를 변수로 할당하거나 함수를 인수로 전달하는 것과 같은 JavaScript의 일급 함수 기능은 Java에는 적용되지 않다.

## 함수가 일급 객체일 때의 장점

- [[고차함수(Higher order function)]]를 만들 수 있다.
- [[콜백 함수(Callback Function)]]을 사용할 수 있다.


## 일급 객체의 조건

### 1. 변수에 할당(assignment)

- [[변수(Variable)]]에 할당은 함수 표현식을 생각하면 된다.

```javascript
const mul = function (num) {
	return num * num;
}
```

### 2. 다른 함수를 인자(argument)로 받음

```javascript
function mul(num) {
  return num*num;
}

// func는 매개변수임, 이름은 아무거나 지정해도 상관없음
function mulNum(func, number) { // 함수, 변수를 파라미터로 받음
  return func(number);
}

let result = mulNum(mul, 3); // 9 // 인자에 함수, 변수를 대입 후 result 인스턴스로 받음
```

- `mul()`에서 매개변수가 하나이기 때문에 `mulNum()`의 매개변수(`number`)가 하나임
- 만약 `mul()`의 매개변수가 2개라면 `mulNum()`의 매개변수도 2개이여야함
- `mulNum()`의 매개변수인 `func`는 위의 `mul()`을 나타내는 것임
- 마지막 `result`변수에 `mulNum()`을 할당하고 인자로 `mul()`을 받아옴
- 이 때 `mulNum()`인자로 다른 함수(`mul()`)를 받아오기 때문에 [[고차함수(Higher Order Function)]]된다.

```javascript
// 이 부분에서 mul을 인자로 받는 mulNum이 고차함수
// mul은 콜백함수이다.
let result = mulNum(mul, 3); 
```