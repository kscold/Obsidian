- 비동기란 요청과 결과가 동시에 일어나지 않는다는 뜻이다.
- 즉 작업을 요청하고 해당 요청에 대한 결과를 기다리지 않아도 된다.
- 그렇기에 작업들을 병렬적으로 처리할 수 있다.
- [[@Async]] [[어노테이션(Annotation)]]을 제공하여 로직의 비동기 처리를 지원한다.

- 아래 그림은 동기/비동기의 작업 처리를 나타낸 그림이다.

![](https://velog.velcdn.com/images/think2wice/post/6cc54152-85bc-4371-9403-f233087d75ba/image.png)

## **클라이언트와 서버의 비동기 통신** 

![](https://blog.kakaocdn.net/dn/sxcOu/btq4eKsIpSZ/kntlVrm6YznC7PKyBHemH1/img.png)

 - 클라이언트에서 서버로 통신하는 메시지를 요청(request) 메시지라고 하며, 서버에서 클라이언트로 통신하는 메시지를 응답(response) 메시지라고 한다.
 
 - 웹에서 화면전환(새로고침) 없이 이루어지는 동작들은 대부분 비동기 통신으로 이루어진다.
 - 비동기통신을 하기위해서는 클라이언트에서 서버로 요청 메세지를 보낼 때, [[HTTP]] body(본문)에 데이터를 담아서 보내야 하고, 서버에서 클라이언트로 응답을 보낼때에도 body(본문)에 데이터를 담아서 보내야 한다. 

 - 즉, 요청 본문 requestBody, 응답 본문 responseBody 을 담아서 보내야 한다.
 - 이때 본문에 담기는 데이터 형식은 여러가지 형태가 있겠지만 가장 대표적으로 사용되는 것이 [[JSON(Java Script Object Notation)]] 이다.

 - 즉, 비동기식 클라이언트-서버 통신을 위해 [[JSON(Java Script Object Notation)]] 형식의 데이터를 주고받는 것이다. 

- 스프링 [[MVC(Mode View Controller)]]에서도 클라이언트에서 전송한 xml데이터나 json 등등 데이터를 컨트롤러에서 DOM 객체나 자바 [[객체(Object)]]로 변환해서 송수신할 수 있다.  

- [[@RequestBody]] 어노테이션과 [[@ResponseBody]] 어노테이션이 각각 [[HTTP]] 요청 body(본문)를 자바 객체로 변환하고 자바객체를 다시 HTTP 응답 바디로 변환해준다.
