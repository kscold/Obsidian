- fs.readFile() [[메서드(Method)]]는 파일을 읽는 데 사용되는 내장 메서드이다.
- 이 메서드는 전체 파일을 [[버퍼(Buffer)]]로 읽는다.

- [[fs.readFileSync()]]([[동기(Synchronous)]]식)과는 다르게 readFile [[메서드(Method)]]는 [[비동기(asynchronous)]]식으로 동작함으로 [[콜백 함수(Callback Function)]]로 메서드 결과를 받아볼 수 있다.


## 문법

### filename

- 읽을 파일의 이름이나 다른 위치에 저장된 경우 전체 경로이다.
### encoding

- 파일의 인코딩을 보유한다.
- 기본값은 'utf8'이다.

### callback

- 파일을 읽은 후 호출되는 [[콜백 함수(Callback Function)]]입니다.
- err는 작업에 실패하면 반환되는 오류이다.
- data는 파일의 내용이다.

```js
fs.readFile( filename, encoding, callback )
```


##  예시

- 만약 에러라면 콜백함수 첫 번째 파라미터(에러)를 통해 에러 내용을 확인할 수 있다.
- 만약 데이터를 성공적으로 가져왔다면 두 번째 파라미터를 통해 파일 내용을 읽어올 수 있다.

```js
const fs = require('fs');

fs.readFile('example1.txt', 'utf8', (err, data) => {
	if (err) {
	    // 만약 파일을 읽어오는데 실패했다면,
	    // 아래는 에러 예시
	    /**
	     * [Error: ENOENT: no such file or directory, open 'example1.txt'] {
	     * errno: -2,
	     * code: 'ENOENT',
	     * syscall: 'open',
	     * path: 'example1.txt'
	     */
	} else {
		console.log(data); // hello
	}  
})
```