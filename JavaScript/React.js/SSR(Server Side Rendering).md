- SSR은 클라이언드(유저)가 페이지를 요청할 때마다 그에 맞는 [[HTML]] 문서를 생성해서 반환한다.

- 이러한 특징이 사용되는 경우는 항상 최신 상태를 유지해야하는 웹 페이지나, 분석 차트, 게시판 등, 사용자의 요청마다 동적으로 페이지를 생성해 다른 내용을 보여주어야 하는 경우에 사용된다.

![](https://blog.kakaocdn.net/dn/bs63oc/btrDPBVg5cw/PNFxOLCBIMHsohJmvESOnK/img.png)


## SSR의 동작 방식

- SSR의 경우 POST -> 리다이렉트 -> GET 방식을 통해 뷰가 렌더링 된다.