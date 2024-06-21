- 자바스크립트는 최상위에 1개의 전역 객체(Global Object)를 가진다. 

## 자바스크립트의 동작과정

- 자바스크립트는 소스코드를 실행하기 전에, 최상위에 위치한 전역 객체(Global Object)를 만는다.

- Math나 Date를 비롯한 몇몇 특별한 [[객체(Object)]]는 제외한다.
- 
- 자바스크립트의 모든 객체와 값들은 이 전역 객체의 아래에 자손(descendants)으로 위치하게 된다.
- 전역 객체는 전체 코드에서 단 1개만 존재할 수 있으며, [[new]] 연산자를 이용하는 등의 방법으로 새롭게 만들 수 없다.

## 전역객체의 종류

- 이 전역객체의 이름은 자바스크립트를 사용하는 환경에 따라 얼마든지 달라질 수 있다.

- 웹 브라우저에서 사용되는 자바스크립트의 전역 객체는 [[window]]라는 이름의 [[객체(Object)]]이다. 
- 한편 자바스크립트를 사용하는 서버인 [[노드(Node.js)]]에서는 [[global]]이라는 이름의 전역 객체가 있다.

- 최신 브라우저에서는 globalThis로 통합되어 있다.


## 전역객체의 특징​

- 자바스크립트에서는 코딩할 때 전역 객체를 생략할 수 있다. 

- 자바스크립트를 이용한 웹 개발 중에 자주 만나게 되는 [[객체(Object)]]들이 있다. 
- [[BOM(Browser Object Model)]]으로 만들어진 screen, navigator, history, location 객체나 document 객체가 바로 그것이다. 

- 그런데 많은 자바스크립트 예제코드에서는 이들 객체 앞에는 아무런 부모객체도 쓰여있지 않은 것을 발견하게 된다.

```js
document.getElementById('test_id').style.display = 'none';

location.href = 'https://www.naver.com/'; 

history.back();
```


- 이 코드가 정상적으로 작동하는 것은 바로 자바스크립트에서 코딩을 할 때 전역객체를 생략할 수 있기 때문에 가능하다. 
- screen, navigator, history, location, document 등의 객체들은, 사실 모두 전역객체 window의 하위에 위치하고 있다.
- 따라서 위 코드는 아래처럼 전역객체인 window를 밝혀 적는다고 하더라도 동일하게 작동합니다.

```js
// 위의 window를 생략한 예시와 똑같이 동작한다.
window.document.getElementById('test_id').style.display = 'none'; 

window.location.href = 'https://www.naver.com/';

window.history.back();
```

- 마찬가지로 웹 개발을 하게 되면 수시로 만나게 되는 setInterval(), setTimeout(), alert(), confirm() 등도 대체로 그 앞에 아무 이름을 적지 않는 경우가 많다.
- 사실 많은 분들이 전역함수로 알고 있는 이들도 실은 전역객체인 window 아래에 있는 [[메서드(Method)]]들입니다. 따라서 아래와 같이 사용하여도 동일하게 작동한다.

```js
window.setInterval("window.alert('test')", 1000); window.setTimeout("window.confirm('test')", 500);
```

- 반대로 많은 예제코드에서 관행적으로 window 객체를 밝혀서 사용하는 함수들도 있다. 
- 팝업을 띄우는 window.open() 함수나 창을 닫는 window.close() 함수로 이들은 역으로 전역객체인 window를 생략해도 잘 작동한다.

```js
open('https://www.naver.com/'); 
close();
```

모든 객체는 전역객체의 속성(property)입니다.

```js
function callFunction(){ 
	alert('함수가 호출되었습니다!'); 
} 

callFunction(); 

window.callFunction();
```

- callFunction();와 window.callFunction();는 모두 실행이 된다. 
- 모든 전역변수와 함수는 사실 window 객체의 속성이다. 객
- 체를 명시하지 않으면 암시적으로 window의 속성으로 간주된다.

```js
var obj = {'callFunction' : function(){ 
	alert('함수가 호출되었습니다!'); 
}} 

obj.callFunction(); 

window.obj.callFunction();
```

- 자바스크립트에서 모든 객체는 기본적으로 전역객체의 속성임을 알 수 있다.

**​**

- ECMAScript에서는 전역객체의 API를 정의해두었다.
- 그 외의 API는 호스트 환경에서 필요에 따라서 추가로 정의하고 있다. 
- 이를테면 웹브라우저 자바스크립트에서는 alert()이라는 전역객체의 메서드가 존재하지만 node.js에는 존재하지 않는다. 
- 또한 전역객체의 이름도 호스트환경에 따라서 다른데, 웹브라우저에서 전역객체는 window이지만 node.js에서는 global이다.

​
## 전역 속성(Global Property)

- 전역 속성은 전역 객체의 속성을 의미한다.
- 애플리케이션 전역에서 사용하는 값들을 나타내기 위해 사용한다.
- 전역 속성은 간단한 값이 대부분이며 다른 속성이나 메서드를 가지고 있지 않다.
​
### Infinity

 - Infinity 속성은 양/음의 무한대를 나타내는 숫자값 Infinity를 가진다.

```js
console.log(window.Infinity); // Infinity 
console.log(3/0); // Infinity 
console.log(-3/0); // -Infinity 
console.log(Number.MAX_VALUE * 2); // 1.7976931348623157e+308 * 2 
console.log(typeof Infinity); // number
```

### NaN

- NaN 속성은 숫자가 아님(Not-a-Number)을 나타내는 숫자값 NaN을 가진다. NaN 속성은 Number.NaN 속성과 같다.
```js
console.log(window.NaN); // NaN 
console.log(Number('xyz')); // NaN 
console.log(1 * 'string'); // NaN 
console.log(typeof NaN); // number
```

### [[undefined]]

- undefined 속성은 기본 자료형 undefined를 값으로 가진다.

```js
console.log(window.undefined); // undefined 
var foo; console.log(foo); // undefined 
console.log(typeof undefined); // undefined
```