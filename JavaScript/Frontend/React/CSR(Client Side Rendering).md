- Next.js가 아닌 React.js는 기본적으로 CSR(Client Side Rendering), [[SPA(Single Page Application)]]방식으로 렌더링한다.

- 이는 모바일 친화적으로 사용될 수 있는 서비스가 필요해서인데, [[MPA(Multiple Page Application)]]를 기반으로 페이지를 렌더링을 할 때마다 모바일에서 성능이 부담이 갔기 때문이다.

- 이때 나온 개념이 [[SPA(Single Page Application)]]으로, 최초 한번 전체 페이지를 로딩한 이후 부터에는 브라우저 내부에서 데이터만 변경해서 사용하는 방식이다.
- 이러한 [[SPA(Single Page Application)]] 방식은 트래픽을 감소시키고 사용자에게 최적화된 환경을 제공할 수 있게 된다.
- 따라서 서버는 단순하게 json 파일만 보내주고, 클라이언트 측에선 초기에 전송받은 [[HTML(Hyper Text Markup Language)]]과 자바스크립트를 통해 렌더링을 수행한다.
- 이러한 과정을 CSR(클라이언트 사이드 렌더링)이라고 한다.