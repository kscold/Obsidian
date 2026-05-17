- fs는 FileSystem의 약자로, fs [[모듈(Module)]]은 [[노드(Node.js)]]에서 파일 입출력 처리를 할 때 사용한다.

- fs [[모듈(Module)]]로 살펴볼 수 있듯 [[비동기(asynchronous)]]로 하되 [[동기(Synchronous)]] 작업처럼 순서를 지키는 것이 동시성과 순서를 동시에 잡는 좋은 방법이다.


## readFile()

```js
fs.readFile('경로', 'utf8', callback); 		// 비동기
fs.readFileSync('경로');  // => <data>		// 동기
```

- fs [[모듈(Module)]]에서 파일을 읽어들이는 [[함수(Function)]]로는 readFile()과 readFileSync()가 있다. 

- "Sync"가 붙은것이 [[동기(Synchronous)]] 버전이다.
- 따라서 [[동기(Synchronous)]] 버전에서는 [[콜백 함수(Callback Function)]]가 필요가 없다.

- 실행 예시를 확인하기 위해 js파일이 있는 폴더에 test.txt 파일을 만들고 "안녕 세상!"이라는 내용을 저장한 다음 아래 코드를 실행한다.

```js
const fs = require("fs");

fs.readFile('./test.txt', 'utf8', (err, data) => {
    console.log(data + "(1)");
});

var data = fs.readFileSync('./test.txt', "utf8"); // 동기 방식으로 읽음 콜백 함수 필요 없음
console.log(data + "(2)");

// >> 안녕 세상!(2)
// >> 안녕 세상!(1)
```

## writeFile()

```js
fs.writeFile(경로, data, [options], callback) 	// 비동기
fs.writeFileSync(경로, data, [options])     	// 동기
```

- fs.writeFileSync()는 filename의 파일에 `[options]`의 방식으로 data 내용을 쓴다. 
- 여기서 writeFile() 은 data 내용을 쓰고 [[콜백 함수(Callback Function)]]를 호출하는 [[비동기(asynchronous)]]적 실행이다.


```js
const fs = require("fs");

fs.writeFile('./test.txt', "수정합니다.", (err) => {
    fs.readFile('./test.txt', 'utf8', (err, data) => {
        console.log(data);
    });
})
```

```js
const fs = require("fs");

fs.writeFileSync('./test.txt', "수정합니다.");
var data = fs.readFileSync('./test.txt', "utf8");
console.log(data);
```


## rename)()

- 파일의 이름 수정할 때 사용한다.

```js
fs.rename(oldPath, newPath, callback) // 비동기
fs.renameSync(oldPath, newPath) // 동기
```


## unlink()

- 파일을 삭제할 때 사용한다.

```js
fs.unlink(Path, callback) // 비동기
fs.unlinkSync(path) // 동기
```