- url [[모듈(Module)]] 안에 URL [[생성자(Constructor)]]가 있다.
- URL은 [[노드(Node.js)]]의 내장 [[객체(Object)]]이기도 해서 [[require()]]할 필요는 없다.


## WHATWG 방식

- 사실 URL [[생성자(Constructor)]]에 주소를 넣어 [[객체(Object)]]로 만들면 주소 부분별로 정리된다.
- username, password, origin, searchParmas 속성이 존재한다.

![[Pasted image 20240328235448.png]]


```js
const url = require('url'); // 현재 줄과

const { URL } = url; // 이 줄이 없이 URL 객체를 실행시켜도 내장객체기 때문에 실행이 됨
const myURL = new URL('http://www.gilbut.co.kr/book/bookList.aspx?sercate1=001001000#anchor');

console.log('new URL():', myURL);
console.log('url.format():', url.format(myURL));
```

- 위의 코드를 실행 시켜보면 [[쿼리스트링(Querystring)]]
를 searchParams([[useSearchParams()]]에서 쓰이는 [[객체(Object)]]와 같음)의 실제 [[객체(Object)]]인 [[URLSearchParams]] 통해 편하게 확인 및 사용할 수 있다.

```bash
> node url.js

new URL(): URL {
  href: 'http://www.gilbut.co.kr/book/bookList.aspx?sercate1=001001000#anchor',
  origin: 'http://www.gilbut.co.kr',
  protocol: 'http:',
  username: '',
  password: '',
  host: 'www.gilbut.co.kr',
  hostname: 'www.gilbut.co.kr',
  port: '',
  pathname: '/book/bookList.aspx',
  search: '?sercate1=001001000',
  searchParams: URLSearchParams { 'sercate1' => '001001000' }, # 쿼리스트링을 구분해줌
  hash: '#anchor'
}

url.format(): http://www.gilbut.co.kr/book/bookList.aspx?sercate1=001001000#anchor
# console.log('url.format():', url.format(myURL));의 결과임
```



