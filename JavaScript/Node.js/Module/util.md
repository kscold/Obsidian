- [[노드(Node.js)]]의 util [[모듈(Module)]]은 [[노드(Node.js)]]의 보조적인 기능 중 유용한 기능만을 모아놓은 모듈이다.


## util의 [[메서드(Method)]]

### util.format() [[매개변수(parameter)]] (format, `[...]`) 

- console.log() 메소드와 비슷한 기능이지만 console.log()는 화면에 출력하고 util.format은 문자열로 반환한다.
- printf와 같은 형식으로 첫 인자를 사용해서 포맷팅된 문자열을 반환한다.
- 플레이스 홀더는 다음과 같은 아규먼트의 값으로 대체 된다.
### util.debug() [[매개변수(parameter)]](string)

- 프로그램의 실행을 멈추고 즉각적으로 string을 출력한다.
### util.log() [[매개변수(parameter)]](string)

- 타임스탬프 시간과 함께 string을 출력한다.
### util.isArray() [[매개변수(parameter)]](object)

- 주어진 object가 Array이면 true, 아니면 false를 리턴한다.
### util.isRegExp() [[매개변수(parameter)]](object)

- 주어진 object가 RegExp이면 true, 아니면 false를 리턴한다.
### util.isDate() [[매개변수(parameter)]](object)

- 주어진 object가 Date이면 true, 아니면 false를 리턴한다.
### util.isError() [[매개변수(parameter)]](object)

- 주어진 object가 Error이면 true, 아니면 false를 리턴한다.
### util.deprecate

- 함수가 deprecated 처리되었음을 알려준다.
- 첫 번째 인자로 넣은 함수를 사용했을 때 경고 메시지가 출력된다.
- 두 번째 인자로 경고 메시지 내용을 넣으면 된다.
- 함수가 조만간 사라지거나 변경될 때 알려줄 수 있어 유용하다.

```js
const util = require('util')

const dontUseMe = util.deprecate((x, y) => {
	console.log(x + y)
}, 'donUseMe 함수는 deprecated되었으니 더 이상 사용하지 마세요!');

dontUseMe(1, 2); // 사용했을 때 경고를 띄움
```
### util.promisify

- [[콜백 함수(Callback Function)]] 패턴을 [[Promise]] 패턴으로 바꿔준다.
- 바꿀 함수를 인자로 제공하면 된다.
- 이렇게 바꾸어두면 [[async await]] 패턴까지 사용할 수 있어 좋다.
- 단, [[콜백 함수(Callback Function)]] 형태가 (error, data) => { } 형식이어야 한다.
- 다행히 노드의 대부분 [[메서드(Method)]]는 이런 형식으로 이루어져 있다.

```js
const util = require('util')
const crypto = require('crypto')

const randomBytesPromis = util.promisfy(crypto.randomBytes); 
// Promise가 아닌 콜백을 Promise로 변환하여 then과 catch를 사용할 수 있도록 만듬

randomBytesPromise(64)
	.then((buf) => {
		console.log(buf.toString('base64'));
	})
	.catch((error) => {
		console.lof(error);
	})
```