- [[노드(Node.js)]]의 기본적인 백엔드 라이브러리를 사용할 때 사용하는 대표적인 라이브러리로 ES6([[ES module]]) 문법이 적용되기 전의 [[CommonJS]] 문법이 적용되어 있다.

- 또한 [[express]]의 [[req]], [[res]] [[객체(Object)]]는 [[HTTP(Hyper Tranfer Protocol)]] 모듈의 [[res]], [[req]] [[객체(Object)]]를 확장한 것이다. 
- 따라서 기존 [[http]] [[모듈(Module)]]의 [[메서드(Method)]]와 Express에서 추가된 [[메서드(Method)]]를 모두 사용할 수 있다.


## 문법

- const express = require('express') 이 코드를 이용해서 express [[변수(Variable)]]에 [[모듈(Module)]] [[객체(Object)]]를 담고 이후 const app = express() 이런식으로 app [[인스턴스(Instance)]]를 선언하는 식으로 사용을 해야한다.
