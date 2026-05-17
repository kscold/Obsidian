- [[노드(Node.js)]]의 request에는 on() [[메서드(Method)]]가 있다.

- on [[메서드(Method)]]는 특정 [[이벤트(event)]]를 [[listen()]] 할 수 있게 해주는 기능을 가지는데, 여기서 인자로 'data'와 [[콜백 함수(Callback Function)]]를 넣어줌으로써 'data event'가 발생할 때마다 해당 [[함수(Function)]]를 수행한다.

- on의 인자로 들어가는 콜백함수에서 데이터 chunk를 인자로 받아서, 만들어놓은 [[배열(Array)]]에 저장한다. 


## 데이터 파싱하기

- request on [[메서드(Method)]]에 인자 'end'와 [[콜백 함수(Callback Function)]]를 넣어서 data event를 [[listen()]]하는 on 기능이 종료되면, 읽어온 데이터를 buffer에 concat하여 string으로 바꿔주고, 특정 인자 (여기서는 '=') 를 기준으로 파싱해준다.

```js
const body = []; 
req.on('data', (chunk) => {
	console.log(chunk);
	body.push(chunk); //body에 chunk 데이터를 넣음 
}); // on 은 특정 event를 listen 할 수 있게 해줌, 여기서는 data event가 발생할 때마다 특정 펑션 수행

req.on('end' , () => {// on에서 읽어와서 body에 저장한 chunk 데이터를 string으로 바꿔서 파싱해줌
	const parsedBody = Buffer.concat(body).toString();
	const message = parsedBody.split('=')[1]; // = 을기준으로 split해서 message에 넣어줌 
});
```


## 파일에 쓰기 / 저장 

- 파일 읽기 / 쓰기 [[객체(Object)]]인 fs 를 가져온다.
- writeFileSync('생성될 파일 이름', 저장할 데이터 );  [[메서드(Method)]]를 통해 파일에 특정 데이터를 저장한다. 

```js
 if(url ==='/message' && method === 'POST' ){
    const body=[]; 
	
	req.on('data', (chunk)=>{
        console.log(chunk);
        body.push(chunk); //body에 chunk 데이터를 넣음
    }); // on 은 특정 event를 listen 할 수 있게 해줌, 여기서는 data event가 발생할 때마다 특정 펑션 수행
    req.on('end' , ()=>{// on에서 읽어와서 body에 저장한 chunk 데이터를 string으로 바꿔서 파싱해 줌
        const parsedBody = Buffer.concat(body).toString();
        const message = parsedBody.split('=')[1]; // = 을기준으로 split해서 message에 넣어줌
	    fs.writeFileSync('message.txt', message);  // 해당 message를 file에 저장 
    });
}
```

## 리다이렉션

- response 객체의 statusCode를 302로 설정해 준다.
- response 객체의 메서드 setHeader에서 ('Location', '리다이렉션 url')을 해준다.

```js
res.statusCode=302; // 302는 redirection을 위한  status code 
res.setHeader('Location', '/'); // location은 /으로 지정 -> 다시 redirection 해서 /로 보내기 
return res.end();
```