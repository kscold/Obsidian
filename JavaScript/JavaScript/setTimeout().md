- 어떤 코드를 바로 실행하지 않고 일정 시간  기다린 후 실행해야하는 경우가 있는데 이럴 때 자바스크립트의 setTimeout() [[함수(Function)]]를 사용할 수 있다.

- 형식은 [[setInterval()]]의 함수와 거의 비슷하다.

## 문법

- 2개의 [[매개변수(parameter)]] 중 첫번째가 [[콜백 함수(Callback Function)]]고 두번째가 ms 초 뒤에 첫번째 콜백 함수를 실행할 함수이다.

```js
setTimeout(function, milliseconds, parameter1, parameter2, ...);
```

```js
setTimeout(() => {
	콜백함수
}, 1000(ms의 정수))
```

- setTimeout() 함수의 첫 번째 매개변수에는 실행시킬 자바스크립트 함수를 넣는다. 
- function에 [[함수(Function)]] 자체를 넣을 수도 있고, 아래와 같이 기명 함수로 넣기도 합니다.

- 아래 코드는 setTimeout() 함수에 기명함수 사용한 예시이다.

```js
function greeting(){
  console.log("Hello World");
}

setTimeout(greeting);
```

- 1초는 1,000 밀리초이다.
- 만약 3초를 기다리게 하고 싶다면, 3000을 두 번째 매개 변수로 전달해야 한다.

- 아래 코드는 3초간 기다리는 setTimeout() 함수 예시이다.

```js
function greeting(){
  console.log("Hello World");
}

setTimeout(greeting, 3000);
```

- 만약 두 번째 매개 변수를 생략한다면, `setTimeout()`은 `function` 함수를 즉시 실행한다.
- 마지막으로, `setTimeout()` 함수에 매개 변수들을 추가로 넣을 수도 있다.

- 아래 코드는 setTimeout()에 추가로 매개변수 전달하는 예시이다.

```js
function greeting(name, role){
  console.log(`Hello, my name is ${name}`);
  console.log(`I'm a ${role}`);
}

setTimeout(greeting, 3000, "Nathan", "Software developer");
```

- 여러분은 아래와 같이 "함수에 바로 매개 변수를 바로 전달하면 안될까?"라고 물을 수도 있습다.

```js
setTimeout(greeting("Nathan", "Software developer"), 3000);
```

- 그럼 자바스크립트는 기다림 없이 function을 실행하게 된다.
- 왜냐하면 첫 번째 매개 변수로 함수 참조(function reference)가 아닌 함수 호출(function call)을 전달했기 때문이다.

- 따라서 함수에 어떤 매개변수들을 전달하고 싶다면, setTimeout() 함수를 통해 전달해야 한다.
- 그러나 setTimeout()에 추가적인 매개 변수를 전달하는 경우는 매우 드물기 때문에 사용을 지양한다.
## clearTimeout()
 
- setTimeout() 함수는 타임아웃 아이디(Timeout ID)라고 불리는 숫자를 반환한다.
- 타임아웃 아이디는 setTimeout() 함수를 호출할 때 마다 내부적으로 생성되는 타이머 [[객체(Object)]]를 가리키고 있다.

- 따라서 이 값을 인자로  함수를 호출하면 기다렸다가 실행될 코드를 취소할 수 있다.

```js
const timeoutId = setTimeout(() => console.log("5초 후에 실행됨"), 5000);
clearTimeout(timeoutId);

// 아무 것도 출력 안 됨
```
