- 웹 서버는 클라이언트로부터 HTTP 요청을 받아 정적인 콘텐츠(HTML 페이지, 이미지, CSS 파일 등)를 제공하는 서버이다.

- 주로 정적인 파일을 제공하며, 동적인 컨텐츠 생성에는 WAS나 다른 서버와의 협력이 필요하다.
- 대표적으로 Apache, Nginx 등이 있다.

- [[리액트(React)]] 및 [[넥스트(Next.js)]]는 프론트엔드 웹 서버(View 담당)이고 [[노드(Node.js)]]는 백엔드 웹 서버(비즈니스 로직 담당)라고 볼 수 있다.

## 프론트엔드 웹 서버와 SPA의 등장 배경

- 일반적인 바닐라 웹 애플리케이션은 [[MPA(Multiple Page Application)]] 방식으로 사용자가 한 페이지에서 다른 페이지로 이동하면 전체 콘텐츠를 로드하는 많은 정적 페이지를 연결하였으나 나중에는 많은 페이지가 연결된 웹 응용 프로그램이 속도가 느려지는 이슈가 있었다.

- 이 문제를 해결하기 위해 개발자는 2000년대 초반에 [[Ajax(Asynchronous JavaScript and XML)]] 를 사용하여 [[MPA(Multiple Page Application)]]를 개선했다.
- 이 기술은 [[MPA(Multiple Page Application)]] 웹 앱 의 성능을 향상시킬 수 있었으나 이것 역시 온라인 페이지의 복잡성 증가와 같은 몇 가지 역효과를 가져왔다.

- 거의 10년 후, [[SPA(Single Page Application)]] 모델이 출시되었고 이것은 [[MPA(Multiple Page Application)]] + [[Ajax(Asynchronous JavaScript and XML)]]의 업그레이드 버전이었다.

- [[SPA(Single Page Application)]]는 페이지 데이터와 레이아웃을 별도로 요청하고 결과를 브라우저에 직접 렌더링할 수 있다.( 예) 유저가 온라인 쇼핑 중에 색상, 브랜드 등으로 선택 항목을 지정하고 페이지를 다시 로드하지 않고도 화면에 렌더링 가능해졌다.)

- 따라서 등장한 [[리액트(React)]]와 [[넥스트(Next.js)]] 모두 웹 프레임워크이며, 서버 측 렌더링([[SSR(Server Side Rendering)]])과 클라이언트 측 렌더링([[CSR(Client Side Rendering)]])을 지원한다.
- 하지만 이 둘을 직접적으로 웹 서버나 [[WAS(Web Application Server)]]와 연결짓는 것은 아니다.


![[Pasted image 20240124171749.png]]

## Next.js의 [[SSR(Server Side Rendering)]]

- [[넥스트(Next.js)]]는 브라우저에 렌더링 할때 기본적으로 [[pre-rendering]]을 해준다.

![](https://blog.kakaocdn.net/dn/LGSXW/btrDPjAr3Mb/SZ9mkhj8wFiZT7Efkvrzt1/img.png)

- 위 사진처럼 [[HTML(Hyper Text Markup Language)]]을 미리 렌더링하고, 그 뒤에 요청이 오면 Chunk 단위로 자바스크립트를 보내주어 이벤트가 작동하게 되는 것이 [[Hydration]]이며, [[넥스트(Next.js)]]에서 사용되는 방법이다.

- 이러한 빌드 과정, [[웹(web)]]페이지 요청 과정이 [[SSR(Server Side Rendering)]]인 것은 아니다.
- 서버에서 [[pre-rendering]]하는 것까지가 [[넥스트(Next.js)]]의 특징인 것이고, [[pre-rendering]]을 동적으로 해서 페이지를 생성하느냐, 정적으로 페이지를 생성하느냐의 차이가 [[SSR(Server Side Rendering)]]과 [[SSG(Static Site Generation)]]의 차이라고 생각하면 된다.