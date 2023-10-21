## req의 객체의 매소드

- **req.app** - app 객체에 접근할 수 있다.
ex) req.app.get('port')

- **req.body** - [[Parsing]] 미들웨어가 만드는 요청의 본문을 해석한 객체

- **req.cookies** - cookie-parser 미들웨어가 만드는 요청의 쿠키를 해석한 객체
- **req.ip** - 요청의 ip 주소
- **req.params** - 라우트 매개변수에 대한 정보가 담긴 객체
- **req.query** - 쿼리스트링에 대한 정보가 담긴 객체
- **req.signedCookies** - 서명된 쿠키들
- **req.get(헤더 이름)** - 헤더의 값