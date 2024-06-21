- instanceof는 비교 연산자로 >, <, == 와 같이 두개의 인자를 받는 연산자로 일반 비교 연산자들과 같이 사용하면 된다.
- 하지만 결과로 리턴하는 것은 typeof와는 성질이 조금 다르다. 

- instanceof는 해당하는 [[변수(Variable)]]가 사용하고 있는 [[prototype]]의 chain을 2번째 인자와 쭉 비교해서 true/false 값을 리턴한다.

- 쉬운말로 하자면, 해당하는 [[변수(Variable)]]의 [[클래스(Class)]]와 비교해서 리턴해주는, java에서 많이 쓰던것과 비슷하다고 볼 수 있다.

## instanceof의 특징
  
```js
var Person = function() {
	this.name = "unikys";
};

var inst = new Person();

inst instanceof Person; // === true
inst instanceof Object; // === true
typeof inst; // === 'object'
```

- instanceof는 [[클래스(Class)]]의 타입을 감지하는 역할을 하고, 위의 코드를 통해 모든 [[클래스(Class)]]는 기본 클래스인 Object를 확장한다는 것을 알 수 있다.

- 하지만 자바스크립트라는 언어의 특징 때문에 그 동작하는 것은 부분부분 매우 다르기도 하다. 
- instanceof가 동작을 다르게 한다기 보다는 자바스크립트라는 언어 자체가 다르게 동작을 하는 것으로 보면 된다.

- 가장 대표적인 예시로 `{}`는 [[new]] Object()와 같은 의미를 하며, `[]`는 [[new]] Array()와 같은 의미를 가지므로 위에 대해서 instanceof를 사용해보면 true가 나올 것이다.

## instanceof의 [[원시 타입(Primitive type)]]

- 하지만 다른 [[원시 타입(Primitive type)]]들에 대해서는 [[클래스(Class)]]로 instanceof를 할 수가 없다.

```js
// 원시 타입들은 불가능
"foo" instanceof String; // === false
"foo" instanceof Object; // === false
true instanceof Boolean; // === false
true instanceof Object; // === false

// 참조 타입들은 가능
[0,1] instanceof Array; // === true
{0:1} instanceof Object; // === true

// 생성자 클래스를 이용한 객체는 가능
var color1 = new String("red");
var color2 = "red";

color1 == color2; // === true
color1 instanceof String; // === true

// "red"가 String인 원시 타입이므로 불가능
color2 instanceof String; // === false
```

- color1의 경우는 String이 맞지만 color2는 [[원시 타입(Primitive type)]] string으로 다르다. 

- 하지만 비교 연산자 `==` 는 true가 나온다. 
- 위에서 [[객체(Object)]] [[리터럴(literal)]]인 `[]`와 `{}`에 대해서 typeof는 둘다 [[객체(Object)]]를 리턴했지만, instanceof는 각각 Array와 Object를 리턴했다.

- 아래는 그래서 조금 다른 예시이다.

```js
"foo".constructor instanceof String; // === false
"foo".constructor === String; // === true
```

- "foo".constructor는 String하고 같지만 String의 [[인스턴스(Instance)]]는 아니다.
- 여기서 크롬의 개발자 콘솔에 String을 쳐보면 왜 그런지 알 수 있다.

```js
function String() { 
	[native code]
}
```

  - 크롬에서 String을 실행하면 위와같이 나온다. 
  - "foo".constructor는 [[함수(Function)]] [[인스턴스(Instance)]]인 것이다.
  - 즉, String 자체는 [[함수(Function)]]의 [[인스턴스(Instance)]]인 것이다.

  * [[원시 타입(Primitive type)]]을 구분할 때에는 typeof를 사용하고 [[클래스(Class)]]의 타입([[참조 타입(Reference Type)]])을 구분할 때에는 instanceof를 사용하면 된다.