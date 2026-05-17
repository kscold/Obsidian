

## 문법

### file

- 텍스트 파일의 상대 경로를 사용한다. 
- 경로는 URL 유형일 수 있다. 
- 파일은 파일 설명자일 수도 있다.
- 두 파일이 같은 폴더에 있는 경우 파일 이름을 따옴표로 묶는다.
### options

- 인코딩 및 플래그를 포함하는 선택적 매개변수이며 인코딩에는 데이터 사양이 포함된다.
- 기본값은 원시 [[버퍼(Buffer)]]를 반환하는 null이며 플래그에는 파일의 작업 표시가 포함된다.
- 기본값은 'r'이다.
- return: 이 메서드는 파일의 내용을 반한다.

```js
fs.readFileSync( path, options )
```


## 예시

```js
const fs = require('fs');

const data = fs.readFileSync('example.txt');

console.log(data); // <Buffer 68 65 6c 6c 6f>
```

- 인코딩 옵션을 넣지 않으면 Buffer 객체(바이너리 데이터)를 반환하기 때문에 utf8 인코딩 옵션을 넣어줍니다.

```js
const fs = require('fs');

const data = fs.readFileSync('example.txt', {encoding:'utf8', flag:'r'})

console.log(data); // hello
```