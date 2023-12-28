
- [[<form>]] 태그에 실어 보낸 데이터는 서버의 컨트롤러가 객체에 담아 받는데 이 객체를 DTO라고 한다.


### DTO의 실행과정
1. 뷰 페이지(.[[mustache]])를 만들고 [[<form>]]태그의 action 속성으로 데이터를 어디로 보낼지(action="경로"), method 속성으로 데이터를 어떻게 보낼지(method="post") 정의한다.
2. 컨트롤러(이름Controller.java)를 만들고 [[@PostMapping]] 방식으로 URI 주소를 연결한다.
3. 전송받은 데이터를 담아 둘 객체인 DTO(이름.java)를 만든다.
4. 컨트롤러에서 폼 데이터를 전송받아 DTO에 담는다. 


