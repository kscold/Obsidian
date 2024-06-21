- JSON은 키와 값(key-value) 한쌍으로 이루어진 데이터 [[객체(Object)]]트를 전달하기 위해 사용하는 개방형 표준 포맷이다. 
- [[HTTP(Hyper Tranfer Protocol)]]통신 시 데이터를 주고받을 때 흔히 쓰이는 데이터 포맷이다.



## JSON [[객체(Object)]] VS JavaScript [[객체(Object)]] [[리터럴(literal)]]

- JSON 객체와 일반 자바스크립트 객체(객체 리터럴이라고도 한다.)의 주요 차이점은 따옴표이다.
- JSON 객체의 모든 키(key)와 문자열 유형 값은 큰따옴표(`"`)로 감싸야 한다.

- 반면 자바스크리트객체 리터럴은 조금 더 유연하다.
- 객체 리터럴을 사용하면 키와 문자열을 큰따옴표로 묶을 필요가 없다.
- 키에 작은따옴표(`'`)를 사용하거나 따옴표 자체를 생략해도 된다.

```json
{
  "name": "Jane Doe",
  "favorite-game": "Stardew Valley", 
  "subscriber": false
}
```

```js
const profile = {
  name: 'Jane Doe',
  'favorite-game': 'Stardew Valley',
  subscriber: false
}
```


### JSON 배열과 JavaScript 배열

- JSON 배열은 JavaScript 배열과 거의 같은 방식으로 작동하며 문자열, 불린(boolean), 숫자 및 기타 JSON 개체를 포함할 수 있다.

```json
[
  {
    "name": "Jane Doe",
    "favorite-game": "Stardew Valley",
    "subscriber": false
  },
  {
    "name": "John Doe",
    "favorite-game": "Dragon Quest XI",
    "subscriber": true
  }
]
```

```js
const profiles = [
  {
    name: 'Jane Doe',
    'favorite-game': 'Stardew Valley',
    subscriber: false
  },
  {
    name: 'John Doe',
    'favorite-game': 'Dragon Quest XI',
    subscriber: true
  }
];
```

## JSON은 사실 문자열

- JSON 또한 객체와 배열이 있다면 일반 자바스크립트 객체 리터럴이나 배열처럼 프로그램에서 사용할 수 없는지 의문이 들 수도 있다.
- 하지 못하는 이유는 JSON은 사실 문자열에 불과하기 때문이다.

- 예를 들어 `jane-profile.json` 또는 `profiles.json`과 같은 별도의 파일에 JSON을 작성하면 해당 파일은 자바스크립트처럼 보이지만 사실 JSON 객체 또는 JSON 배열 형식의 문자열이 담긴다.

- 이때 API에 요청하면 다음과 같은 내용이 반환된다.

```json
{
	"name": "Jane Doe",
	"favorite-game": "Stardew Valley",
	"subscriber": false
}
```

- 텍스트 파일과 마찬가지로 프로젝트에서 JSON을 사용하려면 해당 파일을 구문분석하거나 프로그래밍 언어가 이해할 수 있는 형식으로 변경해야 한다.
- 예를 들어, Python에서 JSON 객체를 구문분석하면 딕셔너리(dictionary)가 생성된다.

- 따라서 자바스크립트에서 JSON을 제대로 객체화 시킬려면 구문분석을 해야한다.

## 브라우저에서 JSON 구문분석하기

- 일반적으로 API를 통해 데이터를 수신하거나 보낼 때 브라우저에서 JSON으로 작업하는 경우가 많다.

- [[fetch()]] 함수 안에서 [[json()]] 메서드를 이용하여 JSON 구문 분석하는 방식이다.
- 반대로 문자열를 JSON으로 변환하여 API 데이터로 보내고 싶다면 [[JSON.stringify()]] 메서드를 사용하면 된다.

- 이렇게 가져온 JSON을 [[fetch()]]로 구문분석하고 JSON.stringify()를 사용해 자바스크립트 객체 리터럴을 JSON 문자열로 변환할 수 있다.
### 브라우저에서 로컬 JSON 파일 작업하기

- 안타깝게도 브라우저에서 로컬 JSON 파일을 로딩하는 것은 가능하지 않고 권장하지 않는다.
- 로컬 파일을 로딩하려고 하면 `fetch`에서 오류가 발생한다.

- 예를 들어 밑에 처럼 농담 데이터가 포함된 JSON 파일이 있다고 가정하자.

```json
[
  {
    "categories": ["dev"],
    "created_at": "2020-01-05 13:42:19.324003",
    "icon_url": "https://assets.chucknorris.host/img/avatar/chuck-norris.png",
    "id": "elgv2wkvt8ioag6xywykbq",
    "updated_at": "2020-01-05 13:42:19.324003",
    "url": "https://api.chucknorris.io/jokes/elgv2wkvt8ioag6xywykbq",
    "value": "Chuck Norris's keyboard doesn't have a Ctrl key because nothing controls Chuck Norris."
  },
  {
    "categories": ["dev"],
    "created_at": "2020-01-05 13:42:19.324003",
    "icon_url": "https://assets.chucknorris.host/img/avatar/chuck-norris.png",
    "id": "ae-78cogr-cb6x9hluwqtw",
    "updated_at": "2020-01-05 13:42:19.324003",
    "url": "https://api.chucknorris.io/jokes/ae-78cogr-cb6x9hluwqtw",
    "value": "There is no Esc key on Chuck Norris' keyboard, because no one escapes Chuck Norris."
  }
]
```

- jokes.json 이 JSON 파일을 구문분석하고 농담 목록을 간단한 HTML 페이지에 만들고 싶다면 어떻해야 할까?

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1" />
    <meta name="viewport" content="width=device-width" />
    <title>Fetch Local JSON</title>
  </head>
  <script>
    fetch("./jokes.json", { mode: "no-cors" }) 
    // disable CORS because path does not contain http(s)
      .then((res) => res.json())
      .then((data) => console.log(data));
  </script>
</html>
```

- 위 예시의 index.html HTML 페이지를 만들고 브라우저에서 여는 경우, 콘솔에는 다음과 같은 메시지가 출력된다.

`Fetch API cannot load file://<path>/jokes.json. URL scheme "file" is not supported`

- 기본적으로 브라우저는 보안상의 이유로 로컬 파일에 대한 액세스를 허용하지 않는다.
- 이런 제한은 사용자의 보안과 안전을 위한 것이기 때문에 이 기본 동작을 변경하려고 하면 안된다.

- 대신 로컬 JSON 파일을 자바스크립트로 변환하는 것이 가장 좋은 방법이다.
- JSON 문법은 JavaScript와 매우 비슷하므로 이런 변환 작업은 어렵지 않다.

- 밑의 코드와 같이 먼저 새 파일을 만들고 JSON 데이터를 변수로 선언하기만 하면 된다.

```js
const jokes = [
  {
    "categories": ["dev"],
    "created_at": "2020-01-05 13:42:19.324003",
    "icon_url": "https://assets.chucknorris.host/img/avatar/chuck-norris.png",
    "id": "elgv2wkvt8ioag6xywykbq",
    "updated_at": "2020-01-05 13:42:19.324003",
    "url": "https://api.chucknorris.io/jokes/elgv2wkvt8ioag6xywykbq",
    "value": "Chuck Norris's keyboard doesn't have a Ctrl key because nothing controls Chuck Norris."
  },
  {
    "categories": ["dev"],
    "created_at": "2020-01-05 13:42:19.324003",
    "icon_url": "https://assets.chucknorris.host/img/avatar/chuck-norris.png",
    "id": "ae-78cogr-cb6x9hluwqtw",
    "updated_at": "2020-01-05 13:42:19.324003",
    "url": "https://api.chucknorris.io/jokes/ae-78cogr-cb6x9hluwqtw",
    "value": "There is no Esc key on Chuck Norris' keyboard, because no one escapes Chuck Norris."
  }
]
```

- jokes.js를 위와 같이 선언한 다음 별도의 스크립트로 HTML 페이지에 추가한다.

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1" />
    <meta name="viewport" content="width=device-width" />
    <title>Fetch Local JSON</title>
  </head>
  <script src="jokes.js"></script>
  <script>
    console.log(jokes);
  </script>
</html>
```

- 이제 코드에서 위의 자바스크립트 [[배열(Array)]]을 자유롭게 사용할 수 있다.

##  Node.js에서 로컬 JSON files로 작업하기

- 하지만컴퓨터에가 설치된 상태에서 하고 싶다면 어떻게 해야 할까
- 대표적으로 2가지 방법이 있다.
### require()로 JSON 파일 구문분석

- 로컬 JSON 파일이 있다면 require()를 사용해 다른 Node.js 모듈처럼 로딩하기만 하면 된다.

```js
const jokes = require('./jokes.json');
```

- JSON 파일이 자동으로 구문분석되고 프로젝트에 이 파일을 바로 사용할 수 있다.

```js
const jokes = require('./jokes.json');

console.log(jokes[0].value); // "Chuck Norris's keyboard doesn't have a Ctrl key because nothing controls Chuck Norris."
```

- 이 작업은 [[동기(Synchronous)]]적으로 처리되므로 전체 파일의 구문분석이 완료할 때까지 프로그램이 일시 중지되기 때문에 JSON 파일이 크면 프로그램 속도가 느려질 수 있다.

- 또한 JSON을 이 방법으로 구문분석하면 파일 전체가 메모리에 로딩되므로 정적 JSON 파일에만 이 방법을 사용하는 것이 좋다.
- 프로그램이 실행되는 동안 JSON 파일이 변경된다면 해당 프로그램을 다시 시작하고 업데이트된 JSON 파일을 구문분석할 때까지 변경 사항에 접근할 수 없다.

### fs.readFileSync()와 JSON.parse()를 사용해 JSON 파일 구문분석

- 두 번째 방법은 Node.js 프로젝트에서 JSON 파일을 구문분석하는 더 보편적인 방법이다.
- fs(파일 시스템) 모듈로 파일을 읽은 다음 JSON.parse()로 구문분석한다.

- fs.readFileSync() [[메서드(Method)]]를 사용해 이 작업을 실행하는 방법에 대해 알아보자.
- 먼저 프로젝트에 fs 모듈을 추가한다.

```js
const fs = require('fs');
```

- 그런 다음 jokes.json 파일의 출력된 내용을 저장할 새 변수를 만들고 fs.readFileSync() [[메서드(Method)]]를 할당한다.

```js
const fs = require('fs');
const jokesFile = fs.readFileSync();
```

- fs.readFileSync() 메서드는 몇 가지 인자를 사용한다.
- 첫 번째 인자는 읽을 파일의 경로이다.

```js
const fs = require('fs');
const jokesFile = fs.readFileSync('./jokes.json');
```

지금 `jokesFile`을 콘솔에 출력한다면, 다음과 같은 내용이 표시됩니다.

```
<Buffer 5b 0a 20 20 7b 0a 20 20 20 20 22 63 61 74 65 67 6f 72 69 65 73 22 3a 20 5b 22 64 65 76 22 5d 2c 0a 20 20 20 20 22 63 72 65 61 74 65 64 5f 61 74 22 3a ... 788 more bytes>
```

이런 내용은 `fs` 모듈이 파일을 읽고 있지만 파일의 인코딩이나 형식을 알지 못한다는 것을 의미합니다. `fs`은 JSON과 같은 텍스트 기반 파일뿐만 아니라 거의 모든 파일을 로딩하는 데 사용될 수 있으므로 해당 파일이 어떻게 인코딩되는지 알려줘야 합니다.

텍스트 기반 파일의 경우 인코딩은 일반적으로 `utf8`입니다.

```js
const fs = require('fs');
const jokesFile = fs.readFileSync('./jokes.json', 'utf8');
```

이제 `jokesFile`를 콘솔에 출력하면 파일의 내용이 표시됩니다.

하지만 로딩이 완료된 파일은 여전히 문자열입니다. 다른 메서드를 사용해 `jokesFile`을 JavaScript 객체 또는 배열로 구문분석해야 하는데, 이때 `JSON.parse()`를 사용하면 됩니다.

```js
const fs = require('fs');
const jokesFile = fs.readFileSync('./jokes.json', 'utf8');
const jokes = JSON.parse(jokesFile);

console.log(jokes[0].value); // "Chuck Norris's keyboard doesn't have a Ctrl key because nothing controls Chuck Norris."
```

메서드 이름에서 알 수 있듯이 `JSON.parse()`는 JSON 문자열을 사용해 JavaScript 객체 리터럴 또는 배열로 구문분석합니다.

위의 `require` 메서드와 마찬가지로 `fs.readFileSync()`는 동기 메서드입니다. 즉, 큰 파일을 처리해야 할 때 프로그램이 느려질 수 있습니다.

`fs.readFileSync()`와 `JSON.parse()`를 사용해 JSON 파일 구문분석하는 방법 또한 파일을 한 번만 읽고 메모리에 로딩합니다. 파일이 도중에 변경된다면 어느 시점에서 파일을 다시 읽어야 합니다. 파일을 쉽게 다시 읽을 수 있는 간단한 기능을 만드는 것이 좋겠죠. 다음과 같이 말이에요.

```js
const fs = require('fs');
const readFile = path => fs.readFileSync(path, 'utf8');

const jokesFile1 = readFile('./jokes.json');
const jokes1 = JSON.parse(jokesFile1);

console.log(jokes1[0].value); // "Chuck Norris's keyboard doesn't have a Ctrl key because nothing controls Chuck Norris."

// jokes.json 파일이 어느 지점에서 변경됩니다 

const jokesFile2 = readFile('./jokes.json');
const jokes2 = JSON.parse(jokesFile2);

console.log(jokes2[0].value); // "Chuck Norris's keyboard is made up entirely of Cmd keys because Chuck Norris is always in command."
```

### `fs.readFile()`과 `JSON.parse()`를 사용해 JSON 구문분석하기

`fs.readFile()` 메서드는 `fs.readFileSync()`와 매우 비슷하지만, 비동기적으로 작동한다는 중요한 차이점이 있습니다. 읽으려고 하는 파일 크기가 크고 로딩 작업이 진행되는 동안 나머지 코드의 실행이 지연되지 않도록 하려면 `fs.readFile()`을 매우 유용하게 사용할 수 있습니다.

간단한 예시를 봅시다.

```js
const fs = require('fs');

fs.readFile('./jokes.json', 'utf8');
```

`jokesFile`과 같은 변수에 메서드를 할당하지 않는다는 점을 제외하면 지금까지 `fs.readFileSync()`를 사용한 작업과 유사합니다. `fs.readFile()`은 비동기식이기 때문에 이후의 코드는 파일 읽기가 끝나기 전에 실행됩니다.

`fs.readFileSync()`와 또 다른 점은 바로 `fs.readFile()`는 JSON 구문분석하는 콜백 함수를 사용해 내부에 있는 JSON을 구문분석한다는 것입니다.

```js
const fs = require('fs');

fs.readFile('./jokes.json', 'utf8', (err, data) => {
  if (err) console.error(err);
  const jokes = JSON.parse(data);

  console.log(jokes[0].value);
});

console.log("이게 먼저 실행됩니다!");
```

그러면 콘솔에 다음 내용이 출력됩니다.

```
이게 먼저 실행됩니다!
Chuck Norris's keyboard doesn't have a Ctrl key because nothing controls Chuck Norris.
```

`fs.readFileSync()`와 마찬가지로 `fs.readFile()`도 파일을 메모리에 로딩하므로 파일이 변경되면 다시 읽어야 합니다.

또한 `fs.readFile()`이 비동기적으로 작동하지만, 작업이 완료되면 읽히는 전체 파일을 메모리로 로딩한다는 점은 `fs.readFile()`과 같습니다. 대용량 파일이 있는 경우 대신 [Node.js 스트림(stream)](https://www.freecodecamp.org/news/node-js-streams-everything-you-need-to-know-c9141306be93/)을 사용하는 것을 추천합니다.

### Node.js에서 [[JSON.stringify()]]를 사용해 JSON 문자열로 변환하기

마지막으로, Node.js로 JSON을 구문분석하면 API 응답으로 JSON을 반환해야 할 수도 있습니다.

다행히 이 작업은 브라우저와 같은 방식으로 작동합니다. `JSON.stringify()`를 사용해 JavaScript 객체 리터럴 또는 배열을 JSON 문자열로 변환하면 됩니다.

```js
const newJoke = {
  categories: ['dev'],
  value: "Chuck Norris's keyboard is made up entirely of Cmd keys because Chuck Norris is always in command."
};

console.log(JSON.stringify(newJoke)); // {"categories":["dev"],"value":"Chuck Norris's keyboard is made up entirely of Cmd keys because Chuck Norris is always in command."}
```

이렇게 지금까지 브라우저 및 Node.js 프로젝트에서 JSON 관련 작업하는 데 필요한 여러 방법에 대해 살펴보았습니다.