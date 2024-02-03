- API에서 데이터를 가져오는 가장 쉬운 방법은 .[[json()]] [[메서드(Method)]]를 통해 [[JSON(Java Script Object Notation)]] 응답을 자바스크립트 [[객체(Object)]] [[리터럴(literal)]] 또는 [[배열(Array)]]로 자동으로 구문분석하는 [[fetch()]]를 사용하는 것이다.

- json() 메서드도 [[Promise]]를 반환하다.

```js
fetch('https://api.chucknorris.io/jokes/random?category=dev')
  .then(res => res.json()) // .json() 메서드는 JSON 응답을 자바스크립트 객체 리터럴로 구문분석합
  .then(data => console.log(data));
```

- 브라우저에 해당 코드를 실행하면 콘솔에 다음과 같은 내용이 표시된다.

```js
{
    "categories": ["dev"],
    "created_at": "2020-01-05 13:42:19.324003",
    "icon_url": "https://assets.chucknorris.host/img/avatar/chuck-norris.png",
    "id": "elgv2wkvt8ioag6xywykbq",
    "updated_at": "2020-01-05 13:42:19.324003",
    "url": "https://api.chucknorris.io/jokes/elgv2wkvt8ioag6xywykbq",
    "value": "Chuck Norris's keyboard doesn't have a Ctrl key because nothing controls Chuck Norris."
}
```

- 해당 내용은 JSON 객체처럼 보이지만 실제로는 자바스크립트 객체 리터럴이기 때문에 프로그램에서 자유롭게 사용할 수 있다.
