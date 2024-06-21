- multer는 파일 업로드를 위해 사용되는 [[노드(Node.js)]]의 [[미들웨어(Middleware)]]이다.
- [[<form>]] 태그의 multipart/form-data 형식으로 단일 및 다중 파일 업로드를 지원하기 때문에 가장 많이 사용된다.


## multer의 기본 설정 

- [[fs]] [[모듈(Module)]]의 [[fs.readFileSync()]] [[메서드(Method)]]를 이용해서 [[서버(Server)]]가 시작되기 전에 [[동기(Synchronous)]]적으로 uploads 파일이 존재하는지 확인하고 없다면 생성한다.

```js
const multer = require('multer');
const fs = require('fs');

  
// 서버에서 sync를 사용하는 경우는 드물지만 서버 시작전에 Sync를 사용하여 uploads 폴더가 없으면 만드는 것은 좋음
try {
	fs.readFileSync('uploads');
} catch (error) {
	console.error('uploads 폴더가 없어 uploads 폴더를 생성합니다.');
	fs.mkdirSync('uploads');
}

const upload = multer({
	storage: multer.diskStorage({
		// 업로드한 파일을 어디에 저장할 것인지 설정
		// 디스크에 저장 또는 메모리에 저장하는 것이 가능
		destination(req, file, done) {
			done(null, 'uploads/'); // done의 첫번째 자리는 보통 에러처리 미들웨어가 들어감
		},
		
		filename(req, file, done) {
			const ext = path.extname(file.originalname); // 확장자 추출
			done(
				null,
				path.basename(file.originalname, ext) + Date.now() + ext 
				// 이름 형식 설정, 날짜를 지정해주지 않으면 이름이 같으면 덮어씌워버림
			);
		},
	}),
	limits: { fileSize: 5 * 1024 * 1024 },
	 // 5 mb 파일만 업로드 가능, 이상이면 400번때 오류가 남

});
```


## 업로드 파일의 갯수에 따른 처리

```js
// /upload uri의 프론트 처리
app.get('/upload', (req, res) => {
	res.sendFile(path.join(__dirname, 'multipart.html'));
});

  
// 하나만 업로드할 때
app.post('/upload', upload.single('image'), (req, res) => {
	// 특정 라우터에서만 일어나기 때문에 upload.single()를 미들웨어처럼 적용
	console.log(req.file);
	res.send('ok');
});

// 여러개를 업로드할 때(이미지가 multiful일 때)
app.post('/upload', upload.array('image'), (req, res) => {
	// 특정 라우터에서만 일어나기 때문에 upload.array()(여러개)를 미들웨어처럼 적용
	console.log(req.files); // 따라서 files에 들어감
	res.send('ok');
});

// 여러개를 업로드할 때(이미지 선택을 여러개 할 때)
app.post(
	'/upload',
	upload.fields(
	{ name: 'image1', limits: 5 }, // 최대 5까까지 한번에 올라감
	{ name: 'image2' },
	{ name: 'image3' }
), (req, res) => {
	// 특정 라우터에서만 일어나기 때문에 upload.fields()(여러개)를 미들웨어처럼 적용
	console.log(req.files.image1); // 따라서 files에 들어감
	console.log(req.files.image2);
	console.log(req.files.image3);
	res.send('ok');
	}
);
```
