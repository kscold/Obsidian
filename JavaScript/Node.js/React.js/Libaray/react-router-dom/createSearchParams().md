- [[React Router]]의 createSearchParams() [[메서드(Method)]]를 사용하여 [[객체(Object)]]형식을 이용하여 편하게  [[쿼리스트링(Querystring)]]를 만들 수 있다.
- 주로 [[useNavigate()]] [[인스턴스(Instance)]] 안에서 이용한다.


## 예시

```js
import { useNavigate, createSearchParams } from 'react-router-dom';

const navigate = useNavigate();

navigate('/login') // 로그인 페이지로 이동

navigate(-1) // 한 단계 전으로 (브라우저의 뒤로가기 한번)
navigate(2) // 두 단계 앞으로 (브라우저의 앞으로가기 두번)

navigate({ // /book?sort=asc&page=3 으로 이동
      pathname: '/book', // useLocation()의 pathname을 넣을 수도 있다. 
      search: `?${createSearchParams({
        sort: 'asc',
        page: 3,
      })}`, // /book?sort=asc&page=3
});
```