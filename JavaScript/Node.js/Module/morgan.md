- 클라이언트에서 어떤 요청이 왔는지 서버에 기록이 되는 [[모듈(Module)]]이다.


## morgan 모듈 사용법

- 전역 [[미들웨어(Middleware)]]로 설정하면 된다.

```js
const express = require('express');
const path = require('path');
const morgan = require('morgan');

  
const app = express();

app.set('port', process.env.PORT || 8080);

app.use(morgan('dev')); // 전역 미들웨어로 사용
```

- dev 말고 'combined'로 설정하면 더 자세하게 로깅을 할 수 있다.