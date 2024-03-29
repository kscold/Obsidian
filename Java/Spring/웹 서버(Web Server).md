- 웹 브라우저와 같은 클라이언트로부터 [[HTTP]] 요청을 받아들이고, HTML 문서와 같은 웹 페이지를 반환하는 컴퓨터 프로그램이다.

- 기본적으로 클라이언트의 [[HTTP]] 통신에 대한 처리를 담당하는 서버로 생각하면 된다.

- 이 때문에 GET같은 HTTP Request가 들어온다면 이에 대응되는 정보를 반환해준다.
- 사용자가 특정 웹페이지에 접속하여 해당 웹페이지의 HTML을 보기를 원한다면 해당 웹페이지에 대한 정보를 웹서버가 받아서 이에 해당하는 HTML을 사용자에게 전달할 것이다.

- 이때 웹서버는 클라이언트와 여러개의 [[Connection]]을 만들어 여러 정보(HTML 템플릿, 이미지, 기타 데이터)등을 병렬적으로 전송하게 된다.

- 또한 POST나 PUT등과 같이 서버에 어떤 정보를 전달하는 클라이언트로 부터 컨텐츠를 전달 받는 것 또한 웹서버의 역할이라고 볼 수 있다.

- 예시로 예로 NGINX와 APACHE가 있다.


## 정적 컨텐츠 vs 동적 컨텐츠

- 정적 컨텐츠는 사용자에 따라 웹페이지가 달리 보여지는게 아니라 모든 클라이언트에게 동일한 웹페이지를 보여지게 하는 것을 의미한다.

![](https://blog.kakaocdn.net/dn/VAH6U/btrpcPOQJU0/5YYQWISyLOdLMuEoSme87k/img.png)

- 동적 컨텐츠는 사용자에 따라 웹페이지가 달리 구성되는 것을 의미한다.

![](https://blog.kakaocdn.net/dn/dgDSI0/btrpiHoFmMG/VKOH4184qn7G8kJtyMbzB0/img.png)

### 동적 컨텐츠의 처리 방식

- 웹서버가 사용자의 정보에 따라 HTML을 구성하기 위해서는 사용자에 따라 특정 정보를 DB에서 불러와 내부 로직을 통해 정보를 처리한 후 HTML에 넣어줘야 할 것이다.

- 하지만 기능에 따라 여러 DB에 다르게 [[쿼리(query)]]를 하고 다르게 데이터를 가공하여 전달 해야할 것이다. 

- 이렇게 되면 웹 서버 안에는 동적 데이터 생성을 하기 위한 부분과 client의 요청을 처리하고 이에 대해 응답하는 부분이 공존하게 되고 이 때문에서 서버에 로드가 커질 수 밖에 없다.

- 따라서 Web server에는 client의 요청과 응답에 대해서만 수행하는 부분만 남기고 다른 동적 컨텐츠를 위한 질의와 구성등은 [[WAS(Web Application Sever)]]로 분리하게 된 것이다.

- 따라서 전체적인 구조도를 보면 다음과 같이 구성되어진다.

![](https://blog.kakaocdn.net/dn/ddu6vR/btrpgzYJeOt/G7yupopF42RFKcwbrCgFeK/img.png)

- 그럼 그냥 서버를 여러개 만들어 로드를 분산 시키는 것일까?

- 그건 아니다. 우리가 사용하는 APP이라는 것은 서버 자체가 가지고 있는 하나의 프로그램을 의미할 수도 있지만 외부 프로그램을 사용해야하는 경우도 존재한다.
- 따라서 우리는 여러 웹 서버들 간의 입출력 형식 등을 맞춰야한다.

- [[CGI(Common Gateway Interface)]]는 공용 게이트웨이 인터페이스의 약자로 웹 서버들간의 정보를 주고받는 일종의 규칙이다.
- 따라서 앞에서 말한 [[WAS(Web Application Sever)]]는 이러한 CGI 규격에 맞게 설계된 서버라고 생각할 수 있다.


## 아파치와 톰캣

### [[아파치(apache)]]

- 아파치란 월드와이드 웹 서버용 소프트웨어이다. 
- 이를 통해 웹을 구축하려 할 때 쉽게 웹 서버를 수행 할 수 있게 된다.

### [[톰캣(Tomcat)]]

- 톰캣은 이러한 아파치르 만든 [[아파치(apache)]] 소프트웨어 재단에서 만든 [[WAS(Web Application Sever)]]이다.

## 예시

![](https://blog.kakaocdn.net/dn/bEURhB/btrVUjbA3z1/KgmEvKDUqKx4cBIVtyQcnK/img.png)

- 위의 이미지는 A가 내가 가지고 있는 컴퓨터라고 가정한다.
- 내가 가진 컴퓨터에는 A게임에 대한 공략법들이 있는 자료들이 있고 이를 친구들이 요청하는 상황이다.

- 이때 내가 가진 컴퓨터는 을(친구들)이 필요한 자료들(A게임에 대한 공략법들이 적혀져 있는 자료)가 있으므로 갑이 된다.

- 반대로 게임에 대한 자료들을 원하는 친구들의 컴퓨터는 을이 된다.

- 이때의 과정을 순서로 나타내면 다음과 같다.
	1. 친구들의 컴퓨터(을)은 내 컴퓨터(갑)에게 자료를 달라고 요청을 하게 된다.
	2. 이에 내 컴퓨터(갑)은 친구들의 컴퓨터(을)에 자료를 응답하게 된다.


- 이 과정의 요청과 응답 과정을 자세하게 살펴본다.

### 자료를 요청할 때(Request)

- 친구들의 컴퓨터(을)은 내 컴퓨터(갑)의 ip주소를 토대로 요청(request)한다.
- 이때 친구들의 컴퓨터(을)은 내 컴퓨터(갑)가 어디 있는지 알아야 요청이 가능하다.

### 자료를 응답할 때(Response)

- 내 컴퓨터(갑)은 친구들의 컴퓨터(을)에게 요청된 자료에 대해서 응답(response)한다.
- 이때 내 컴퓨터(갑)은 친구들의 컴퓨터(을)가 어디있는 상관 없이 요청에 대한 응답만 하면 된다.

- 이처럼 을이 원하는 자료가 가지고 있는 컴퓨터를 웹서버라고 한다.

- 웹 서버와 통신하기 위해 필요한 것은 다음과 같습니다.
	1. 웹 서버의 ip주소 = 웹 서버에게 자료를 요청하기 위해 어디 있는지 ip주소를 알아야 한다.
	2. [[URL(Uniform Resource Locator)]]의 주소 = 웹 서버의 어떤 자료가 필요한지 표시하기 위해 URL이 필요하다.