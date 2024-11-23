- 브라우저 지원에서, 모든 주요 브라우저들은 ES module문법을 지원하고 [[리액트(React)]]와 Vue.js 같은 프레임워크에서 [[import]]/[[export]]를 사용할 수 있다. 이 프레임워크들은 바벨과 같은 트랜스파일을 사용해서 import/export를 이전 [[노드(Node.js)]] 버전이 기본적으로 지원하는 [[require()]]로 바꿔서 컴파일합니다.


## 노드에서 ES modeles의 사용

- mjs 확장자 대신 js 확장자를 사용하면서 ES module을 사용하려면 package.json에 type: "module" 속성을 넣으면 된다.


## ES modules는 JS의 표준이고 CommonJS는 Node.js의 기본값

- ES module 형태는 JS [[모듈(Module)]] system을 표준화하기 위해서 만들어졌다. 
- 재사용을 위해 JS코드를 캡슐화하는 표준형식이 되었다.

- 반면에 [[CommonJS]] [[모듈(Module)]] 시스템은 Node.js에 내장되어있다.
- Node.js에서의 Es module이 도입되기 전에는 [[CommonJS]]가 Node.js 모듈의 표준이었다.

- 결과적으로, 많은 양의 Node.js 라이브러리와 모듈은 CommonJS로 작성되어있다.  

- JS 모듈의 표준이 되는 것 외에도 ES 모듈 문법은 [[require()]]에 비해서 더 읽기 쉽다.
- 클라이언트에서 JS를 작성하는 웹 개발자는 동일한 문법 덕에 [[노드(Node.js)]] [[모듈(Module)]]을 사용하는데에 문제가 없다.