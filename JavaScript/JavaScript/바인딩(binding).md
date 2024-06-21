- [[setTimeout()]]에 [[메서드(Method)]]드를 전달할 때처럼, [[객체(Object)]] [[메서드(Method)]]를 [[콜백 함수(Callback Function)]]으로 전달할 때 '[[this]] 정보가 사라지는' 문제가 생긴다.

## 사라진 [[this]]

- [[this]] 정보가 사라지는 문제가 종종 나타난다.
- [[객체(Object)]] [[메서드(Method)]]가 객체 내부가 아닌 다른 곳에 전달되어 호출되면 [[this]]가 사라진다.

- 밑에 코드는 `setTimeout`을 사용한 아래 예시에서 `this`가 어떻게 사라지는지 알아본다.

``` js
let user = {
	firstName: "John",
	sayHi() {
		alert(`Hello, ${this.firstName}!`);
	}
}

setTimeout(user.sayHi, 1000);// Hello, undefined!
```

- 위의 예시에서 `this.firstName`이 "John"이 되어야 하는데, alert창에는 `undefined`가 출력된다.

- 이렇게 된 이유는 `setTimeout`에 객체에서 분리된 함수인 `user.sayHi`가 전달되기 때문이다. 
- 위 예시의 마지막 줄은 다음 코드와 같다.

```js
let f = user.sayHi
setTimeout(user.sayHi, 1000);// user 컨텍스트 잃어버림
```

- 브라우저 환경에서 [[setTimeout()]] [[메서드(Method)]]는 조금 특별한 방식으로 동작한다. 
- 인수로 전달받은 함수를 호출할 때, [[this]]에 [[window]]를 할당한다.(Node.js 환경에선 `this`가 타이머 객체가 되는데, 여기선 중요하지 않으므로 넘어간다).
- 따라서 위 예시의 `this.firstName`은 `window.firstName`가 되는데, `window` 객체엔 `firstName`이 없으므로 `undefined`가 출력된다.
- 다른 유사한 사례에서도 대부분 `this`는 `undefined`가 된다.

특히 위 방법처럼 객체 메서드를 실제 메서드가 호출되는 곳(예시에선 `setTimeout` 스케줄러)으로 전달하는 것은 아주 흔하기 때문에 밑에는 바인딩하는 방법을 알아보자.

## 래퍼

- 가장 간단한 해결책은 래퍼 함수를 사용하는 것이다.

``` js
let user = {
	firstName: "John",
	sayHi() {
		alert(`Hello, ${this.firstName}!`);
	}
}

setTimeout(function() {
	user.sayHi(); // Hello, John!
}, 1000);
```

- 위 예시가 의도한 대로 동작하는 이유는 외부 [[렉시컬 환경(Lexical Environment)]]에서 `user`를 받아서 보통 때처럼 메서드를 호출했기 때문이다.

- 따라서 [[화살표 함수(Arrow function)]]를 사용하여 아래와 같이 변경할 수도 있다.

``` js
setTimeout(() => user.sayHi(), 1000); // Hello, John!
```

- 이렇게 코드를 작성하면 간결해져서 보기는 좋지만, 약간의 취약성이 생긴다.
- `setTimeout`이 트리거 되기 전에(1초가 지나기 전에) `user`가 변경되면, 변경된 객체의 메서드를 호출하게 된다.

``` js
let user = {
	firstName: "John",
	sayHi() {
		alert(`Hello, ${this.firstName}!`);
	}
}

setTimeout(() => user.sayHi(), 1000); 

// 1초가 지나기 전에 user의 값이 바뀜
user = { sayHi() { alert("또 다른 사용자!"); } };

// setTimeout에 또 다른 사용자!
```

- 따라서 두 번째 방법인 [[bind()]] [[키워드(Keyword)]]를 사용하면 이런 일이 발생하지 않는다.

## 부분적용

- 지금까진 `this` 바인딩에 대해서만 이야기했다.
- `this` 뿐만 아니라 인수도 바인딩이 가능하다.
- 인수 바인딩은 잘 쓰이진 않지만 가끔 유용할 때가 있습니다.
- `bind`의 전체 문법은 다음과 같다.

```jsx
let bound = func.bind(context, [`arg1`],[`arg2`], ...);
```

- `bind`는 컨텍스트를 `this`로 고정하는 것 뿐만 아니라 함수의 인수도 고정한다.

- 곱셈을 해주는 함수 `mul(a, b)`를 예시로 들어보자.

```jsx
function mul(a, b) {
	return a * b;
}
```

- `bind`를 사용해 새로운 함수 `double`을 만든다.

```jsx
function mul(a, b) {
	 return a * b;
} 

let double = mul.bind(null, 2);

alert(double(3)); // = mul(2, 3) = 6
alert(double(4)); // = mul(2, 4) = 8
alert(double(5)); // = mul(2, 5) = 10
```

- `mul.bind(null, 2)`를 호출하면 새로운 함수 `double`이 만들어진다. 
- `double`엔 컨텍스트가 `null`, 첫 - 번째 인수는 `2`인 `mul`의 호출 결과가 전달된다.
- 추가 인수는 ‘그대로’ 전달된다.

- 이런 방식을 부분 적용(partial application)이라고 부른다.
- 부분 적용을 사용하면 기존 함수의 매개변수를 고정하여 새로운 함수를 만들 수 있다.

- 위 예시에선 `this`를 사용하지 않았다는 점에 주목하시기 바란다. 
- `bind`엔 컨텍스트를 항상 넘겨줘야 하므로 `null`을 사용했다.

- 부분 적용을 사용해 3을 곱해주는 함수 `triple`을 만들어보자.


```` `function` `mul``(```a`,` b```)` `{`   `return` a `*` b`;` `}`  _`let` triple `=` `mul``.``bind``(``null``,` `3``)``;`_  `alert``(` `triple``(``3``)` `)``;` `// = mul(3, 3) = 9` `alert``(` `triple``(``4``)` `)``;` `// = mul(3, 4) = 12` `alert``(` `triple``(``5``)` `)``;` `// = mul(3, 5) = 15` ````

그런데 부분 함수는 왜 만드는 걸까요?

가독성이 좋은 이름(`double`, `triple`)을 가진 독립 함수를 만들 수 있다는 이점 때문입니다. 게다가 `bind`를 사용해 첫 번째 인수를 고정할 수 있기 때문에 매번 인수를 전달할 필요도 없어지죠.

이 외에도 부분 적용은 매우 포괄적인 함수를 기반으로 덜 포괄적인 변형 함수를 만들수 있다는 점에서 유용합니다.

함수 `send(from, to, text)`가 있다고 가정해 봅시다. 객체 `user` 안에서 부분 적용을 활용하면, 전송 주체가 현재 사용자인 함수 `sendTo(to, text)`를 구현할 수 있습니다.

## [컨텍스트 없는 부분 적용](https://ko.javascript.info/bind#ref-17)

인수 일부는 고정하고 컨텍스트 `this`는 고정하고 싶지 않다면 어떻게 해야 할까요?

네이티브 `bind`만으로는 컨텍스트를 생략하고 인수로 바로 뛰어넘지 못합니다.

다행히도 인수만 바인딩해주는 헬퍼 함수 `partial`를 구현하는 건 쉽습니다.

아래와 같이 말이죠.

[](https://ko.javascript.info/bind# "실행")

[](https://ko.javascript.info/bind# "샌드박스에서 열기")

````` _`function` `partial``(```func`,` `...`argsBound```)` `{`   `return` `function``(``` `...`args ```)` `{` `// (*)`     `return` `func``.``call``(``this``,` `...`argsBound`,` `...`args`)``;`   `}` `}`_  `// 사용법:` `let` user `=` `{`   firstName`:` `"John"``,`   `say``(```time`,` phrase```)` `{`     `alert``(`````` `` ` ```[``` `${`time`}` ```]` ``` `${``this``.`firstName`}` ````:` `` `${`phrase`}` ```!``` ` `` ``````)``;`   `}` `}``;`  `// 시간을 고정한 부분 메서드를 추가함` user`.`sayNow `=` `partial``(`user`.`say`,` `new` `Date``(``)``.``getHours``(``)` `+` `':'` `+` `new` `Date``(``)``.``getMinutes``(``)``)``;`  user`.``sayNow``(``"Hello"``)``;` `// 출력값 예시:` `// [10:00] John: Hello!` `````

`partial(func[, arg1, arg2...])`을 호출하면 래퍼(`(*)`)가 반환됩니다. 래퍼를 호출하면 `func`이 다음과 같은 방식으로 동작합니다.

- 동일한 `this`를 받습니다(`user.sayNow`는 `user`를 대상으로 호출됩니다).
- `partial`을 호출할 때 받은 인수(`"10:00"`)는 `...argsBound`에 전달됩니다.
- 래퍼에 전달된 인수(`"Hello"`)는 `...args`가 됩니다.

전개 구문 덕분에 이 모든 과정이 쉬워졌습니다.

lodash 라이브러리의 [_.partial](https://lodash.com/docs#partial)을 사용하면 컨텍스트 없는 부분 적용을 직접 구현하지 않아도 됩니다.

## [요약](https://ko.javascript.info/bind#ref-18)

`func.bind(context, ...args)`는 `this`가 `context`로 고정되고 인수도 고정된 함수 `func`을 반환합니다.

`bind`는 보통 객체 메서드의 `this`를 고정해 어딘가에 넘기고자 할 때 사용합니다. `setTimeout`에 넘길 때 같이 말이죠.

기존 함수의 인수 몇 개를 고정한 함수를 _부분 적용(partially applied)_ 함수 또는 _부분(partial)_ 함수라고 부릅니다.

부분 적용은 같은 인수를 여러 번 반복하고 싶지 않을 때 유용합니다. `send(from, to)`라는 함수가 있는데 `from`을 고정하고 싶다면 `send(from, to)`의 부분 함수를 구현해 사용하면 됩니다.