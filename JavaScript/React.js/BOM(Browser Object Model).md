- 자바스크립트가 브라우저와 소통하기 위해서 만들어진 모델이다.
- 모든 개별 객체들은 최상위 객체([[전역 객체(Global Object)]])인 [[window]] 아래 존재한다.

- 웹 페이지 내용을 제외한 [[웹(Web)]] 브라우저 창에 포함된 모든 [[객체(Object)]] 요소들을 의미한다.

- [[window]] [[속성(Property)]]에 속하며 document 문서가 아닌, window를 제어한다.

## BOM의 종류

| 객체 종류     | 역할                                          |
| --------- | ------------------------------------------- |
| window    | [[전역 객체(Global Object)]]로 각 프레임별로 하나씩 존재한다. |
| location  | url 주소에 대한 정보를 제공한다.                        |
| document  | 현재 문서에 대한 정보이다.                             |
| navigator | 브라우저의 정보를 제공, 주로 호환성 문제를 위해 사용한다.           |
| history   | 브라우저의 **방문 기록** 정보를 제공한다.                   |
| screen    | 브라우저의 **외부 환경**에 대한 정보를 제공한다.               |