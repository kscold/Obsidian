- window [[객체(Object)]]는 두 가지 역할을 한다.

1. 브라우저 안의 모든 요소들이 소속된 [[객체(Object)]]로, 최상위에 있기 때문에 어디서든 접근이 가능하다고 해서 [[전역 객체(Global Object)]]라고도 부른다. 

2. 일반적으로 우리가 열고 있는 브라우저의 창(browser window)을 의미하고, 이 창을 제어하는 다양한 [[메서드(Method)]]를 제공한다.


## 전역 객체로써 window

- window 객체는 문자 그대로 window라는 이름으로 접근할 수 있다.

```js
window
```

- 브라우저 콘솔에서 그냥 window를 찍어보면, Window {parent: Window, opener: global, top: Window, length: 3, frames: Window, …}  이런 값을 얻을 수 있고, 펼쳐보면 생각보다 뜸을 들이면서 펼쳐지는데 이 속에 무수하게 많은 프로퍼티들이 존재하는 것을 확인할 수 있다. 

- 사실 이 안에 모든 [[속성(Property)]]들을 다 외울 필요도 없고 이해할 필요도 없지만, 한 가지 재미있는 사실로 우리가 작성하는 코드들은 대부분 다 이 window 객체의 [[속성(Property)]]이 된다는 사실을 기억해두자.

```js
var myName = 'Bigtop';

function getMyName() {
    return myName;
}

console.log(window.myName); // Bigtop
console.log(window.getMyName()); // Bigtop
```

- 이렇게, 우리가 [[var]] [[키워드(Keyword)]]로 [[변수(Variable)]]를 선언하거나 함수를 선언하면, 다 이 window 객체의 [[속성(Property)]]이 된다.

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
