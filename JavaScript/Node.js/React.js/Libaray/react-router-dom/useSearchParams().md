- react-router-dom([[React Router]])에서 [[쿼리스트링(Querystring)]] 값을 가져올 수 있는 [[Hooks]]으로는 [[useLocation()]], useSearchParams 두개가 있다.

- [[useLocation()]]은 location [[객체(Object)]]에 너무 많은 [[객체(Object)]] 정보를 반환하므로 request할 때 [[쿼리스트링(Querystring)]] 자체만 뽑기에는 useSearchParams()가 [[쿼리스트링(Querystring)]]의 [[객체(Object)]]만 [[배열(Array)]]형태로 반환하기 때문에 사용이 더 쉽다.

- 실질적으로 뽑아낼 때는 [[쿼리스트링(Querystring)]] 반환된 배열 [[인스턴스(Instance)]].get("key") 형태로 [[쿼리스트링(Querystring)]]을 뽑아낸다.

## 문법

```jsx
const [searchParams, setSearchParams] = useSearchParams();
```

- searchParams 는 URLSearchParams 객체이면서 쿼리 스트링을 다루기 위한 여러 [[메서드(Method)]]를 제공한다.
- setSearchParams 는 인자에 [[객체(Object)]], 문자열을 넣어주면 현재 url 의 쿼리스트링을 변경하는 기능을 제공한다.

## 메서드
### getter

- 값을 읽어오는 메서드의 종류이다.
#### searchParams.get(key) 

- 특정한 key의 value를 가져오는 메서드, 해당 key 의 value 가 두개라면 제일 먼저 나온 value 만 리턴한다.
#### searchParams.getAll(key)

- 특정 key 에 해당하는 모든 value 를 가져오는 메서드이다.
#### searchParams.toString()

- 쿼리 스트링을 string 형태로 리턴한다.

### setter

- 값을 변경하는 메서드의 종류이다.
- searchParams을 변경하는 메서드로 값을 변경해도 실제 url 의 쿼리 스트링은 변경되지 않는다.
- 이를 변경하려면 setSearchParams에 searchParams를 인자로 전달해야 한다.
- 혹은 [[createSearchParams()]]를 사용하여 쿼리스트링값을 파싱할 수 있다.
#### searchParams.set(key, value)

- 인자로 전달한 key 값을 value 로 설정, 기존에 값이 존재했다면 그 값은 삭제된다.
#### searchParams.append(key, value)

- 기존 값을 변경하거나 삭제하지 않고 추가하는 방식이다.


## 예시

```jsx
import React from 'react';
import { useSearchParams } from 'react-router-dom';


const ListPage = () => {
	const [queryParams] = useSearchParams();
	
	const page = queryParams.get('page') ? parseInt(queryParams.get('page')) : 1;
	const size = queryParams.get('size') ? parseInt(queryParams.get('size')) : 10;
	
	return (
		<div className="p-4 w-full bg-white">
			<div className="text-3xl font-extrabold">
				Todo List Page Component --- {page} --- {size}
			</div>
		</div>
	);
};


export default ListPage;
```