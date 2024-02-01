- 문자열를 JSON으로 변환하여 API로 데이터를 보낼 때 사용하는 [[메서드(Method)]]이다.

- API로 데이터를 보내기 위해서는 위에 생성한 객체 리터럴을 [[JSON(Java Script Object Notation)]] 문자열로 변환해야 한다.
- 바로 JSON.stringify()이다.


## 예시

- 아래와 같이 자바스크립트에서 생성한 [[객체(Object)]]가 있다.

```js
const newJoke = {
  categories: ['dev'],
  value: "Chuck Norris's keyboard is made up entirely of Cmd keys because Chuck Norris is always in command."
};
```

- 밑의 코드는 방금 생성한 [[객체(Object)]]를 JSON.stringify()를 사용하여 JSON으로 변환하는 예시이다.

```js
const newJoke = {
  categories: ['dev'],
  value: "Chuck Norris's keyboard is made up entirely of Cmd keys because Chuck Norris is always in command."
};

console.log(JSON.stringify(newJoke)); // {"categories":["dev"],"value":"Chuck Norris's keyboard is made up entirely of Cmd keys because Chuck Norris is always in command."}

console.log(typeof JSON.stringify(newJoke)); // string
```

- 이 예시에서 [[객체(Object)]] [[리터럴(literal)]]을 JSON 문자열로 변환하는 것처럼 JSON.stringify()는 [[배열(Array)]]에서도 똑같이 작동한다.
- 마지막으로 JSON 문자열로 변환된 데이터를 POST 요청과 함께 API로 다시 보내면 된다.

```js
const newJoke = {
  categories: ['dev'],
  value: "Chuck Norris's keyboard is made up entirely of Cmd keys because Chuck Norris is always in command."
};

fetch('https://api.chucknorris.io/jokes/submit', { // fake API endpoint
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
  },
  body: JSON.stringify(newJoke), // JavaScript 객체 리터럴을 JSON 문자열로 변환하기
})
  .then(res => res.json())
  .then(data => console.log(data))
  .catch(err => {
    console.error(err);
  });
```