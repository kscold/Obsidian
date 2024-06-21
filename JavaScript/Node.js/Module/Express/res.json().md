- res.json()은 [[JSON(Java Script Object Notation)]]이 아닌 것도 [[JSON(Java Script Object Notation)]] 형식으로 바꾸어서 보내준다. 

- 만약 [[express]]가 아니라 일반 적인 [[노드(Node.js)]]의 [[http]] [[모듈(Module)]]이였다면 [[res.writeHead()]]와 [[res.end()]]를 통해 [[JSON.stringify()]] [[메서드(Method)]]를 사용해야 되었을 것이다.

- 따라서 res.json() [[메서드(Method)]]는 손 쉽게 content-type 헤더를 application/JSON으로 고정하고 [[JSON(Java Script Object Notation)]]형식으로 만들어준다.

- 내부적으로 res.json()도 마지막에 [[res.send()]]를 호출한다.  

- 따라서 [[웹 서버(Web Server)]]보다는 api [[서버(Server)]]를 만들 때 많이 사용된다.