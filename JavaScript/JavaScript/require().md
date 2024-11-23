- [[노드(Node.js)]]에서는 require [[메서드(Method)]]를 통해 외부 [[모듈(Module)]]을 가져올 수 있다.
- 첫번째 require에서는 파일을 로드하지만, 이후 과정부터는 [[모듈(Module)]]을 사용하면서 미리 로드되어 있는 파일의 [[객체(Object)]]를 메모리의 캐싱해놓고 사용한다. (require.cache 부분이다.)

- require [[메서드(Method)]]는 [[노드(Node.js)]]가 지역 [[객체(Object)]]에 추가한 [[메서드(Method)]]로서 [[매개변수(parameter)]]로 추가할 모듈의 파일 경로값을 받는다.

- require()는 [[module.exports()]]를 반환하기 때문에 터미널에서 node 명령어로 실행했을때 빈 [[객체(Object)]](`{}`)를 반환하는 것을 확인할 수 있다.
- node require.main 명령어로 어떤 파일을 실행한건지 확인가능하다.

- 최신 문법인 [[import]]의 경우 파일의 코드 최상단에 위치해야하지만 require은 코드 제일 상단애 위치할 필요는 없다.
- 서로 참조하는 경우인 순환참조가 발생하는 경우 [[노드(Node.js)]]가 알아서 빈 [[객체(Object)]](`{}`)로 바꿔버린다.


## 문법

```js
const foo = require('파일 경로');
```

- 위의 코드와 같이 require [[메서드(Method)]]는 파일 경로를 문자열로 [[매개변수(parameter)]]로 받는다.

```js
const { foo, bar } = require('파일 경로');
```

- 또한 [[객체(Object)]]이기 때문에 [[비구조화 할당]]을 통하여 원하는 [[변수(Variable)]]나 [[메서드(Method)]]를 뽑을 수 있다.