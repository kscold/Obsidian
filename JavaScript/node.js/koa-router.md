- index.js 에서 보통 설정한다.

```javascript
const Koa = require('koa');
const Router = require('koa-router');

const app = new Koa(); // Koa 라이브러리를 사용하는 기초적인 인스턴스를 생성
const router = new Router(); // kao-router 인스턴스를 생성

// 라우터 설정
router.get('/', (ctx) => { // 첫번째 함수 라우트의 경로, 두 번째 파라미터 미들웨어 함수
  ctx.body = '홈';
});

router.get('/about', (ctx) => {
  ctx.body = '소개';
});

// app 인스턴스에 라우터 적용
app.use(router.routes()).use(router.allowedMethods());

app.listen(4000, () => {
  console.log('Listening to port 4000');
});
```

- router.get()의 첫 번째 파라미터에는 라우트의 경로가 들어가고, 두 번째 파라미터에는 해당 라우트에 적용할 미들웨어 함수를 넣는다.