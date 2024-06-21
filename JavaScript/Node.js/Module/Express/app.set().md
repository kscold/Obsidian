- 주로 [[노드(Node.js)]]의 Koa나 [[express]]와 같은 웹 프레임워크에서 사용한다.
- ctx.set() 형식으로 사용하여 [[HTTP(Hyper Tranfer Protocol)]] 응답 헤더를 설정한다.

- 쉽게 생각하면 [[변수(Variable)]]와 거의 비슷한데 [[모듈(Module)]]에서 사용할 수 있을 [[속성(Property)]]을 정의한다고 생각하면 된다.
- 따라서 [[app.set()]]으로 설정한 것은 [[app.get()]]으로 받아 사용할 수 있다.

- app.set()의 경우 전체 [[속성(Property)]]에 정의를 하는 것이기 때문에 모든 [[미들웨어(Middleware)]]에서 접근이 가능하다.


## Express에서 set() 사용

```js
ctx.set('Last-Page', Math.ceil(postCount / 10)); 
```

- 위 부분에서는 마지막 페이지 번호를 HTTP 응답 헤더에 설정한다.
- 여기서 `'Last-Page'`은 헤더 이름이고, `Math.ceil(postCount / 10)`는 페이지 수를 계산한 값이다.

- 이 코드는 문서 갯수를 10으로 나누고 올림한 결과를 마지막 페이지로 설정한다.
- 페이징에서는 일반적으로 한 페이지당 아이템의 수를 정의하며, 이 예에서는 10개로 가정한다.

- `Last-Page`로 header 설정을 하였기 때문에 postman에서 확인이 가능하다.