- [[React Router]]에서 [[쿼리스트링(Querystring)]]을 사용할 때 쓰는 [[Hooks]]이다.

- useLoaction()은 loaction [[객체(Object)]]를 반환한다.
- 이 객체는 사용자가 보고 있는 페이지의 정보를 지니고 있다.

## loaction 객체 [[속성(Property)]]

- 이 객체에는 다음과 같은 값들이 있다.
### pathname

- 현재 주소의 경로이다.
- ? 뒤의 [[쿼리스트링(Querystring)]]은 제외한다.
### search

- 맨 앞의 ? 문자를 포함한 [[쿼리스트링(Querystring)]] 값이다.
### hash

- 주소의 # 문자열 뒤의 값이다.
- 주로 [[History API]]가 지원되지 않는 구형 브라우저에서 클라리언트 라우팅을 사용할 때 쓰는 해시 라우터에서 사용한다.
### state

- 페이지로 이동할 때 임의로 넣을 수 있는 상태 값이다.
### key

- location [[객체(Object)]]의 고유 값이다.
- 초기에는 default이며 페이지가 변경될 때마다 고유의 값이 생성된다.
