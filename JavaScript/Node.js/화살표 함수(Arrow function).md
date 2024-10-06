- 익명 함수처럼 [[함수(Function)]]를 정의할 때 사용한다.

- 화살표함수로 선언하면 [[메서드(Method)]] [[바인딩(binding)]]을 생략할 수 있다.
- 값을 바로 반환하고 싶다면 코드 블록 `{}`를 생략하면 된다.
- [[this]]가 필요한 경우에는 [[function]]를 사용하고 [[this]]가 필요 없는 경우에는 전부 화살표 함수로 [[함수(Function)]]를 선언한다고 생각하면 편하다.

- 주로 [[콜백 함수(Callback Function)]]를 정의 할때 사용한다.


## [[콜백 함수(Callback Function)]]로 사용되는 경우

- [[비동기(asynchronous)]] 함수인 [[setTimeout()]]에도 [[콜백 함수(Callback Function)]] 형태로 화살표 함수가 들어간다.

```jsx
setTimeout(funtion() {
	console.log('hello world');
}, 1000);
```


```jsx
setTimeout(() => {
	console.log('hello world');
}, 1000);
```


## [[function]] [[키워드(Keyword)]]로 대체할 수 없는 경우

- [[function]] [[키워드(Keyword)]]를 대체할 수 없는 것은 용도가 다르기 때문이다. 
- 무엇보다 서로 가리키고 있는 [[this]] 값이 다르다.

```jsx
function BlackDog() {
	this.name = '흰둥이';
	
	return {
		name: '검둥이',
		bark: function() {
			console.log(this.name + ': 멍멍!');
		}
	}
}

const blackDog = new BlackDog();
blackDog.bark(); 

// >> 검둥이: 멍멍!

function WhiteDog() {
	this.name = '흰둥이';
	
	return {
		name: '검둥이',
		bark: () => {
			console.log(this.name + ': 멍멍!');
		}
	}
}

const whiteDog = new WhiteDog();
whiteDog.bark(); 

// >> 흰둥이: 멍멍!
```

- 위의 코드에서 [[function]]을 사용했을 때는 검둥이가 나타나고, 화살표 함수를 사용했을 때는 흰둥이가 나타난다. 

- 또한 [[addEventListener()]]를 예시로 [[this]]의 차이를 설명할 수 있다.

```js
button.addEventListener('click', funtcion() {
	console.log(this.textContent); // 정상 작동
});
```

- 위의 경우 정상적으로 현재의 button [[객체(Object)]]의 클릭에 매핑된다.
- 그러나 아래의 화살표 함수로 선언한 경우 [[this]]가 [[인스턴스(Instance)]]를 가르키므로 제대로 동작하지 않는다.

```js
button.addEventListener('click', () => {
	console.log(this.textContent); // 에러
});
```

- 따라서 위의 코드를 [[화살표 함수(Arrow function)]]로 버튼 [[객체(Object)]]를 동작하게 하기 위해서는 [[이벤트(event)]] [[객체(Object)]]를 받아 e.target.value로 접근할 수 있다.

```js
button.addEventListener('click', (e) => {
	console.log(e.target.textContent); // 정상 작동
});
```

- 일반 [[함수(Function)]]는 자신이 종속된 [[객체(Object)]]를 [[this]]로 가리키며, 화살표 함수를 자신이 종속된 [[인스턴스(Instance)]]를 가리킨다.


## 화살표 함수에서  값 바로 반환하기

- 화살표 함수는 값을 연산하여 바로 반환해야 할 때 사용하면 가독성을 높일 수 있다.

```jsx
function twice(value) {
	return value * 2;
}

const triple = (value) => (value * 3) // () 생략 안한 코드

const triple = (value) => value * 3; // () 생략 한 코드
```

- 따로 `{}`를 열어 주지 않으면, 연산한 값을 그대로 반환한다는 의미이다.
- `()` 소괄호는 바로 리턴을 한다는 의미이다.

- [[객체(Object)]]를 바로 리턴하는 경우에는 소괄호가 필수적이다.

```js
const obj = (x, y) => ({ x, y }) // 객체 리턴
```

