- 모듈이란 특정한 기능을 하는 [[함수(Function)]]([[메서드(Method)]])나 [[변수(Variable)]]의 집합을 의미한다.

- 브라우저에서는 다른 js 파일을 불러올 때 html `<script>`태그를 이용해서 불러올 수 있었다.  
- [[노드(Node.js)]]에는 [[HTML(Hyper Text Markup Language)]]이 없는데 js 파일에서 다른 js 파일이나 모듈을 [[CommonJS]]를 통해 가지고 와서 사용한다.

- [[노드(Node.js)]]는 자바스크립트 코드를 모듈로 만들 수 있다.
- 모듈로 만들면 여러 프로그램에서 재사용이 가능하다.
- 대표적으로 [[리액트(React)]]가 포함하고 있는 [[웹팩(webpack)]]이 모듈로 이루어진 번들러이다.

![[Pasted image 20240311030142.png]]


## 외장 모듈

- 개발자들이 만들어 놓은 것이다.
- npm으로 다운 받은 후 [[require()]]로 사용한다.

## 내장 모듈

- Node.js 설치시 그 안에 제공되는 모듈이다.

## 모듈 불러와 사용하기

```js
const 모듈이 담기는 변수명 = require('모듈이름')
```

- node_modules에 저장되어 있는 모듈을 `require('모듈이름')`으로 불러올 수 있다.

- [[노드(Node.js)]]에는 기본적으로 포함하고 있는 내장 모듈들이 있다.  
- 그 중 하나로 파일 시스템과 관련된 모듈인 File System module을 사용하려면 아래와 변수 `fs`에 fs를 담으면 된다.

## 모듈을 재사용하는 방법

- 아래 코드와 같이 var.js를 만들고 다른 파일에서 사용할 수 있도록 [[module.exports()]]에 [[객체(Object)]]를 대입해준다.

```js
// var.js
const odd = '홀수입니다.';
const even = '홀수입니다.';

module.exports = { // 다른 파일에서 쓸 수 있도록 설정 및 shorthand property 적용
	odd,
	even,
}
```

- 아래 예시는 [[require()]] 구문([[import]]의 예전 버전)을 통해 파일 안에 [[객체(Object)]]을 가져와서 출력하는 코드이다.

```js
// func.js
const value = require('./var'); // // var.js 파일을 import함

console.log(value); 
// >> { odd:'홀수입니다.', even:'홀수입니다.' }
```