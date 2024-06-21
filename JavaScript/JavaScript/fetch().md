- 원격 API를 간편하게 호출할 수 있도록 브라우저에서 제공하는 [[비동기(asynchronous)]] [[함수(Function)]]이다.
- [[Promise]]를 반환하는 [[Web API]] 중 하나이다.

- 브라우저에서 fetch() 함수를 지원하기 이 전에는 클라이언트 단에서 직접 [[HTTP(Hyper Tranfer Protocol)]] 요청하고 응답을 받는 게 상당히 복잡해서 이러한 라이브러리를 사용하는 것이 합리적이었다.
- 그러나 요즘에는 브라우저에서 내장된 fetch() 함수를 이용하면 대부분의 경우 충분하기 때문에 오히려 이러한 라이브러리를 사용하는 것이 자바스크립트 번들(bundle) 파일의 크기만 늘려서 낭비가 될 수 있다.

## 문법

- fetch() 함수는 첫번째 인자로 URL, 두번째 인자로 옵션 [[객체(Object)]]를 받고, [[Promise]] 타입의 [[객체(Object)]]를 반환한다.
- 반환된 객체는, API 호출이 성공했을 경우에는 응답(response) 객체를 resolve하고, 실패했을 경우에는 예외(error) 객체를 reject한다.

```js
fetch(url, options)
	.then((response) => console.log("response:", response))
	.catch((error) => console.log("error:", error));
```

- 옵션(options) 객체에는 HTTP 방식(method), HTTP 요청 헤더(headers), HTTP 요청 전문(body) 등을 설정해줄 수 있다.
- 응답(response) 객체로 부터는 [[HTTP 응답 상태(status)]], HTTP 응답 헤더(headers), HTTP 응답 전문(body) 등을 읽어올 수 있다.

- 참고로 fetch() 함수는 엄밀히 말해, 브라우저의 [[window]] 객체에 소속되어 있기 때문에 window.fetch()로 사용되기도 한다.

### GET

- 원격 API에 있는 데이터를 가져올 때 쓰이는 GET 방식의 HTTP 통신 방법이다.
- fetch() 함수는 디폴트로 GET 방식으로 작동하고 GET 방식은 요청 전문을 받지 않기 때문에 옵션 인자가 필요가 없다.

```js
fetch("https://jsonplaceholder.typicode.com/posts/1").then((response) =>
  console.log(response)
);
```

- 응답 객체를 통해 응답 상태가 `200 OK`인 것을 금방 알 수 있다.

```json
Response {status: 200, ok: true, redirected: false, type: "cors", url: "https://jsonplaceholder.typicode.com/posts/1", …}
```

- 대부분의 REST API들은 [[JSON(Java Script Object Notation)]] 형태의 데이터를 응답하기 때문에, 응답(response) 객체는 json() 메서드를 제공합니다.

```js
fetch("https://jsonplaceholder.typicode.com/posts/1")
  .then((response) => response.json())
  .then((data) => console.log(data));
```

- 이 메서드를 호출하면, 응답(response) 객체로 부터 JSON 포맷의 응답 전문을 자바스크립트 객체로 변환하여 얻을 수 있다.

```json
{
  "userId": 1,
  "id": 1,
  "title": "sunt aut facere repellat provident occaecati excepturi optio reprehenderit",
  "body": "quia et suscipit↵suscipit recusandae consequuntur …strum rerum est autem sunt rem eveniet architecto"
}
```

단순히 특정 API에 저장된 데이터를 보여주는 웹페이지나 애플리케이션에서는 GET 방식의 HTTP 통신으로 충분할 것입니다.

## POST 호출

- 원격 API에서 관리하고 있는 데이터를 생성해야 한다면 요청 전문을 포함할 수 있는 POST 방식의 HTTP 통신이 필요할 것입니다.

동일한 API를 대상으로 이번에는 새로운 포스팅를 생성하기 위해서 `fetch()` 함수를 사용해보겠습니다. `method` 옵션을 `POST`로 지정해주고, `headers` 옵션을 통해 JSON 포맷을 사용한다고 알려줘야 하며, 요청 전문을 JSON 포맷으로 직렬화화여 가장 중요한 `body` 옵션에 설정해줍니다.

```js
fetch("https://jsonplaceholder.typicode.com/posts", {
  method: "POST",
  headers: {
    "Content-Type": "application/json",
  },
  body: JSON.stringify({
    title: "Test",
    body: "I am testing!",
    userId: 1,
  }),
}).then((response) => console.log(response));
```

이번에는 응답 객체를 통해 응답 코드가 `201 Created`인 것을 알 수 있습니다.

```js
Response {type: "cors", url: "https://jsonplaceholder.typicode.com/posts", redirected: false, status: 201, ok: true, …}
```

```js
fetch("https://jsonplaceholder.typicode.com/posts", {
  method: "POST",
  headers: {
    "Content-Type": "application/json",
  },
  body: JSON.stringify({
    title: "Test",
    body: "I am testing!",
    userId: 1,
  }),
})
  .then((response) => response.json())
  .then((data) => console.log(data));
```

마찬가지 방법으로 응답 객체의 `json()` 메서드를 호출하면 응답 전문을 객체 형태로 얻을 수 있습니다.

```js
{title: "Test", body: "I am testing!", userId: 1, id: 101}
```

## [](https://www.daleseo.com/js-window-fetch/#put-delete-%ED%98%B8%EC%B6%9C)PUT, DELETE 호출

GET과 POST만큼은 아니지만, 원격 API에서 관리하는 데이터의 수정과 삭제를 위해서 PUT과 DELETE 방식의 HTTP 호출을 해야할 때가 있습니다.

PUT 방식은 `method` 옵션만 `PUT`으로 설정한다는 점 빼놓고는 POST 방식과 매우 흡사합니다.

```js
fetch("https://jsonplaceholder.typicode.com/posts/1", {
  method: "PUT",
  headers: {
    "Content-Type": "application/json",
  },
  body: JSON.stringify({
    title: "Test",
    body: "I am testing!",
    userId: 1,
  }),
})
  .then((response) => response.json())
  .then((data) => console.log(data));
```

```js
{title: "Test", body: "I am testing!", userId: 1, id: 1}
```

DELETE 방식에서는 보낼 데이터가 없기 때문에, `headers`와 `body` 옵션이 필요가 없습니다.

```js
fetch("https://jsonplaceholder.typicode.com/posts/1", {
  method: "DELETE",
})
  .then((response) => response.json())
  .then((data) => console.log(data));
```

```json
{}
```

## [](https://www.daleseo.com/js-window-fetch/#%EB%B3%B4%EB%84%88%EC%8A%A4-%EC%82%AC%EC%9A%A9%EC%84%B1-%EA%B0%9C%EC%84%A0)[보너스] 사용성 개선

`fetch()` 함수는 사용법이 아주 간단하지만, 계속 사용하다보면 똑같은 코드가 반복된다는 것을 느끼실 것입니다. 예를 들어, 응답 데이터을 얻기 위해서 `response.json()`을 매번 호출하거나, 데이터를 보낼 때, HTTP 요청 헤더에 `"Content-Type": "application/json"`로 설정해주는 부분은 지루하게 느껴질 수 있습니다. 뿐만 아니라, 기존에 사용하시던 라이브러리와 비교해봤을 때, `fetch()` 함수의 Promise 기반의 API가 좀 투박하다고 느끼실 수도 있습니다.

이럴 때는 `fetch()` 함수를 직접 사용하시기 보다는, 본인 입맛에 맞게 별도의 함수나 모듈로 빼서 사용하시기를 추천드립니다. 저같은 경우에는 프로젝트의 상황에 맞게 다음과 같이 async/await 키워드를 이용하여 HTTP 방식별로 비동기 함수를 작성하고 모듈화하여 사용하곤 합니다.

```js
async function post(host, path, body, headers = {}) {
  const url = `https://${host}/${path}`;
  const options = {
    method: "POST",
    headers: {
      "Content-Type": "application/json",
      ...headers,
    },
    body: JSON.stringify(body),
  };
  const res = await fetch(url, options);
  const data = await res.json();
  if (res.ok) {
    return data;
  } else {
    throw Error(data);
  }
}

post("jsonplaceholder.typicode.com", "posts", {
  title: "Test",
  body: "I am testing!",
  userId: 1,
})
  .then((data) => console.log(data))
  .catch((error) => console.log(error));
```

## 추가적인 라이브러리
 
 - request, axios, jQuery와 같은 라이브러리일 것이다.

