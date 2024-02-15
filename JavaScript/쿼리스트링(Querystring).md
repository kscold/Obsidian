- 쿼리스트링은 주소 뒷부분에 ? 문자열 이후 key=value 값을 정의하며 &로 구분하는 형태이다.

- [[React Router]]에서 쿼리스트링을 사용할 때는 [[URL 파라미터]]와 달리 Route [[컴포넌트(component)]]를 사용할 때 별도로 설정해야 되는 것은 없다.
- [[React Router]]에서 [[쿼리스트링(Querystring)]]을 사용할려면 [[useLoaction()]]을 이용한다.

## URL 형식

```
/articles?page=1&keyword=react
```