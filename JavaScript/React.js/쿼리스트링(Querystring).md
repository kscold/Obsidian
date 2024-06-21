- 쿼리스트링은 주소 뒷부분에 ? 문자열 이후 key=value 값을 정의하며 &로 구분하는 형태이다.

- 주로 [[URL 파라미터]]의 경우 정적인 데이터를 저장하고 쿼리스트링에 경우 유동적인 데이터를 저장한다.
- 예시를 들면 오늘 상품 페이지 위치와 같은 데이터이다.

- [[React Router]]에서 쿼리스트링을 사용할 때는 [[URL 파라미터]]와 달리 Route [[컴포넌트(Component)]]를 사용할 때 별도로 설정해야 되는 것은 없다.
- [[React Router]]에서 [[쿼리스트링(Querystring)]]을 사용할려면 [[useLocation()]]을 이용한다.


## URL 형식

```
/articles?page=1&keyword=react
```

- [[리액트(React)]]에서 쿼리스트링을 추출하는 방법은 대표적으로 [[useLocation()]]과 [[useSearchParams()]]기 있다.
- 리다이렉트 개념이 많이 들어감으로 [[<Navigate>]]에 많이 쓰인다.