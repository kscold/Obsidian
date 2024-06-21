- window [[객체(Object)]]는 브라우저에 의해 자동으로 생성되며 웹 브라우저 창(window)을 나타낸다.
- 따라서 window는 브라우저의 [[객체(Object)]]이지 자바스크립트의 [[객체(Object)]]가 아니다.

- window [[객체(Object)]]는 크게 2가지 역할을 하는데, 브라우저 안의 모든 요소들이 소속된 [[객체(Object)]]로, 최상위에 있기 때문에 어디서든 접근이 가능하다고 해서 [[전역 객체(Global Object)]]라고도 부른다. ([[BOM(Browser Object Model)]]) 

- 따라서 변수나 메서드를 생성해도 window [[객체(Object)]] 안에서 제어가 되는 것이며, 이 창을 제어하는 다양한 [[메서드(Method)]]를 제공한다.


## [[전역 객체(Global Object)]]로써 window

- window 객체는 문자 그대로 window라는 이름으로 접근할 수 있다.

```js
window
```

- 브라우저 콘솔에서 그냥 window를 찍어보면, Window {parent: Window, opener: global, top: Window, length: 3, frames: Window, …}  이런 값을 얻을 수 있고, 펼쳐보면 생각보다 뜸을 들이면서 펼쳐지는데 이 속에 무수하게 많은 프로퍼티들이 존재하는 것을 확인할 수 있다. 

- window 안의 모든 [[속성(Property)]]들을 다 외울 필요가 없고, 작성하는 코드들은 대부분 window [[객체(Object)]]의 [[속성(Property)]]이 된다.

```js
var myName = 'Bigtop';

function getMyName() {
    return myName;
}

console.log(window.myName); // Bigtop
console.log(window.getMyName()); // Bigtop
```

- 이렇게, [[var]] [[키워드(Keyword)]]로 [[변수(Variable)]]를 선언하거나 함수를 선언하면, 다 이 window 객체의 [[속성(Property)]]이 된다.

- 그럼에도 불구하고 우리는 그냥 변수와 함수를 선언하고서 앞에 window를 붙이지 않는데, 말 그대로 window는 전역 객체로 페이지 내에 있는 모든 객체를 다 포함하고 있기 때문에 window는 그냥 생략이 가능한 특징이 있다. 

- 그래서 특별한 경우를 제외하면 사실상 window 객체를 직접적인 사용할 일은 드물다고 볼 수 있다.

- 하지만 [[let]]과 [[const]] 키워드로 선언한 변수는 블록 [[스코프(Scope)]]이기 때문에 window 객체 내부의 블록에서 선언된 것으로 평가되어 전역 객체의 [[속성(Property)]]으로 활용되기는 어렵다.
- 이 부분은 scope의 개념을 잘 이해하고 있다면 충분히 이해할 수 있는 부분이다.

```js
let myName = 'Bigtop';

console.log(window.myName); // undefined
```

## 브라우저의 창으로써 window

- window 객체는 브라우저의 창을 대변하고, 다양한 [[메서드(Method)]]를 통해 이 창을 제어할 수 있다.

- window.close(), window.open() [[메서드(Method)]]를 활용하면, 창을 열거나 닫을 수 있고, window.innerWidth, window.innerHeight 같은 [[속성(Property)]]에 접근하면 창의 너비와 높이 등을 확인할 수도 있다.

### window 메서드

```js
let val;

val = window.outerHeight; // 전체 높이
val = window.outerWidth; // 전체 넓이

val = window.innerHeight; // 화면만의 높이
val = window.innerWidth; // 화면만의 넓이

val = window.screenY; // 현재 세로 스크롤 좌표
val = window.screenX; // 현재 가로 스크롤 좌표

val = window.location; // url에 대한 정보

// window.location.href = 'https://www.google.com'; // 바로 이동 가능

// window.history.forward() // 앞으로 가기
// window.history.back() // 뒤로 가기
// window.navigater // 브라우저의 정보가 포함되어 있는 객체

console.log(val);
```
