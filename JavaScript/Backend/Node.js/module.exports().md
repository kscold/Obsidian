- [[노드(Node.js)]]는 [[모듈(Module)]]의 특성을 이용하여 module.export [[메서드(Method)]]를 통해 다른 파일에서 [[객체(Object)]]를 사용할 수 있게 해준다.
- [[객체(Object)]]를 module.exports() 했다면 다른 파일에서 [[require()]]하여 사용하면 된다.

- module.export는 [[노드(Node.js)]]가 만들어낸 [[속성(Property)]]인데 빈 [[객체(Object)]](`{}`)가 대입되어 있다.

- exports() [[메서드(Method)]]도 존재하는데 단순히 module.exports()와 같은 역할을 하는 별칭이다.
- [[객체(Object)]] 형식이나 [[배열(Array)]] 형식으로 넘겨줄 수 있다.

- 다른 파일에서 받은 [[객체(Object)]]를 또 다시 넘겨줄 수 있으나, 주의해야할 점은 파일당 한번만 module.exports() [[메서드(Method)]]를 사용할 수 있다.

- [[노드(Node.js)]]에서 [[this]]는 module.exports()와 같다.


## 객체를 module.exports() 하기

```js
// var.js
const odd = '홀수입니다.';
const even = '홀수입니다.';

module.exports = { // 다른 파일에서 쓸 수 있도록 설정 및 shorthand property 적용
	odd,
	even,
}

// func.js
const { odd, even } = require('./var');

function checkOddOrEven(number) {
	if (number % 2) {
		return odd;
	} else {
		return even;
	}
}

module.exports = checkOddOrEven;
```


## exports.객체 형식으로 사용

- `module.exports === exports === {}`의 관계가 유지되어 있다면 exports.객체 형식으로 바꿔서 사용해도 가능하다.
- 그러나 module.exports에 [[함수(Function)]]([[메서드(Method)]])가 수출되면 exports 와 참조가 관계가 끊기므로(`!==`) 이 부분에 대해서 조심해야 한다.

- 또한 한 파일 안에 exports.객체 형식을 사용하는 순간부터는 module.exports 형식을 사용할 수 없다.

```js
const odd = '홀수입니다.';
const even = '홀수입니다.';

exports.odd = odd;
exports.even = even;
```


## module.exports VS exports

- module.exports(), exports 둘 다 모듈을 만들어 내보내는 방법이다.

- module.exports가 원조이다.
- exports는 일종의 축약형(shortcut)이다. 

- 따라서 exports는 module.exports를 참조한다.
- exports라는건 module.exports를 가르키고 있는 변수인셈이다.

```js
let exports = module.exports = {}
```    

```js
// hello.js
exports.anything = function() {
	console.log('I am anything');
};

// hello-runner.js
const hello = require('./hello');

console.log(hello);	// {anything: Function}
hello.anything();	// I am anything
```

```js
// hello.js
module.exports.anything = function() {
  console.log('I am anything');
};

// hello-runner.js
const hello = require('./hello');

console.log(hello);	// {anything: Function}
hello.anything();	// I am anything
```

- module.exports와 exports 둘 다 모듈을 만들어 내보내는 역할을 하므로 아래 두 개의 코드는 언뜻보기엔 이상이 없어 보이지만 [[require()]]로 불러와 출력해보면 다른 결과를 가진다.

```js
//hello.js 
module.exports = {a: 2}

//hello2.js
const hello = require('./hello');
console.log(hello);	// {a:2}
```

```js
//hello.js 
exports = {a: 2}

//hello2.js
const hello = require('./hello');
console.log(hello);	// {}
```

- exports에 직접 참조하는 패턴을 쓰게되면 나타나는 문제이다. 
- 이런 현상이 발생한 이유는 [[변수(Variable)]]에 [[객체(Object)]]의 주소를 넣어주게 된 것이라 [[import]]를 하면 빈 객체만 출력이 되게 된다.  

- 모듈화해서 exports에 직접 넣어주게 되면 변수에 할당한거지 module.exports에 넣어주는게 아니다.