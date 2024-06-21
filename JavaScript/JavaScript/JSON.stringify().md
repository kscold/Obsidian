- JSON.stringify() [[메서드(Method)]]는 자바스크립트 값이나 [[객체(Object)]]를 JSON 문자열로 변환한다.
- 즉, 문자열를 [[JSON(Java Script Object Notation)]]으로 변환하여 API로 데이터를 보낼 때 사용하는 [[메서드(Method)]]이다.

- API로 데이터를 보내기 위해서는 위에 생성한 [[객체(Object)]] [[리터럴(literal)]]을 [[JSON(Java Script Object Notation)]] 문자열로 변환해야 한다.

- 바로 JSON.stringify()이다.


## 문법

```js
  JSON.stringify(value[, replacer[, space]])
```

- 선택적으로, replacer를 [[함수(Function)]]로 전달할 경우 변환 전 값을 변형할 수 있고, [[배열(Array)]]로 전달할 경우 지정한 [[속성(Property)]]만 결과에 포함한다.
### value

- [[JSON(Java Script Object Notation)]] 문자열로 변환할 값이다.

### replacer(Optional)

- 문자열화 동작 방식을 변경하는 [[함수(Function)]]이다.
- 혹은 JSON 문자열에 포함될 값 객체의 속성들을 선택하기 위한 화이트리스트(whitelist)로 쓰이는 String 과 Number [[객체(Object)]]들의 [[배열(Array)]]이다.
- 이 값이 null 이거나 제공되지 않으면, 객체의 모든 속성들이 JSON 문자열 결과에 포함된다.

### space(Optional)

- 가독성을 목적으로 JSON 문자열 출력에 공백을 삽입하는데 사용되는 String 또는 Number [[객체(Object)]]이다.

- 이것이 Number라면, 공백으로 사용되는 스페이스(space)의 수를 나타낸다.
- 이 수가 10 보다 크면 10 으로 제한된다. 1 보다 작은 값은 스페이스가 사용되지 않는 것을 나타낸다.
- 이것이 String이라면, 그 문자열(만약 길이가 10 보다 길다면, 첫번째 10 개의 문자)이 공백으로 사용된다. 
- 이 [[매개변수(parameter)]]가 제공되지 않는다면(또는 null 이면), 공백이 사용되지 않는다.


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