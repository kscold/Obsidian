- URL 문자열을 입력하면 URL [[객체(Object)]]를 만든다.
- 'format()'의 반대이다.


## URL 입력 값 찾아오기

-  예를 들어 `http://localhost/?id=HTML](http://localhost/?id=HTML` 에서  id=HTM 부분이 [[쿼리스트링(Querystring)]]이다.

- 이 [[쿼리스트링(Querystring)]]에 따라 다른 정보를 볼 수 있다.

- `http://host:8000/?name=Tom`의 url이라고 가정했을 때 아래 코드처럼 url.parse.query를 하면 `name=Tom`처럼 [[쿼리스트링(Querystring)]] 값만 나온다.

```js
let query = url.parse(req.url, true).query;
```

- 이후 query에 .name을 하면 querystring에서 name의 값이 나온다.

```js
    response.end('Hello ' + query.name + '\n');
```

