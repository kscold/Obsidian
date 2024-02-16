- 톰캣은 웹 서버가 이해하지 못하는 자바 파일들을 컴파일하고 html 문서로 만들어 주는 역할을 한다.

- 스프링 부트는 내장으로 톰캣을 따로 설치할 필요가 없이 바로 프로그램을 실행할 수 있다.

- 즉 [[웹 서버(Web Server)]]([[아파치(apache)]])는 [[웹 서버(Web Server)]]는 요청된 정적 파일들을 응답하고 톰켓은 웹 서버가 이해하지 못하는 [[JSP]] 파일 또는 자바 코드로 이루어진 파일을 컴파일해서 html 파일로 번역해서 [[웹 서버(Web Server)]]에게 넘긴다.

## 톰캣과 [웹 서버(Web Server)]에 대한 이해

- 우리가 흔히 볼 수 있는 [[아파치(apache)]]는 [[웹 서버(Web Server)]]의 일종이다.

- 아래 그림은 정적 자원을 요청할 경우이다.

![](https://blog.kakaocdn.net/dn/Nvass/btrVT6RaH6Z/IomSgS6WiJro0PeAP5KS3k/img.png)

- 정적 자원이 요청될 경우 웹서버(아파치)는 해당하는 자료를 찾아서 웹 브라우저에 응답한다.

- 정적자원이 아닌 [[JSP]] 파일 또는 자바 코드가 적혀져 있는 자원을 요청하는 경우는 어떻게 처리할까?
- 웹 서버는 해당 파일을 이해하지 못하기 때문에 제어권을 [[톰캣(tomcat)]]넘겨 처리하도록 한다.

![](https://blog.kakaocdn.net/dn/23sGX/btrVTV3n4Dj/DV7bfabezpUGYGMQna3azk/img.png)

-  톰캣은 해당 파일을 자바 파일 모두를 컴파일 하고, 컴파일된 데이터를 html 파일에 덮어씌운다.

![](https://blog.kakaocdn.net/dn/Bpm7a/btrVOyBBxhR/2F12aUppXAOU1Wro5L3YLK/img.png)

- 톰켓은 아파치에게 html 파일을 넘겨주고 [[웹 서버(Web Server)]]([[아파치(apache)]])는 웹 브라우저에게 해당하는 ~.html파일을 응답한다.

![](https://blog.kakaocdn.net/dn/b5pLNF/btrVUicHvcX/cjGwFSLaqpDxxn1ktKrYp0/img.png)



