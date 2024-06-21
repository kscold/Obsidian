- [[React Router]] 라이브러리의 useNavigate()는 프로그래밍 방식으로 [[<Link>]] [[컴포넌트(Component)]]를 사용하지 않고 다른 페이지로 이동해야 하는 상황에 사용하는 [[Hooks]]이다.

- 방향을 처리(navigate)할 수 있는 [[함수(Function)]]를 반환한다.

## 문법

```jsx
navigate("../success",  { replace: true });
```

- navigate(0), navigate(-1) 과 같은 값을 넣어 페이지 새로고침, 히스토리의 뒤로가기 버튼과 같은 동작을 시킬 수도 있다.
- 옵셔널한 두 번째 인자로 [[객체(Object)]]를 전달하면서 replace와 [[state]]를 설정할 수 있다.

### replace

- boolean 값이다.
- history에 이력을 남길지 여부를 설정할 수 있다.

- true는 history 안남기지 않는다. 
- 즉, 뒤로 가기 불가능하다.

- false는 history 남긴다. 
- 즉, 뒤로 가기 가능하다.
- default 값이다.

### state

- {state: 전달할 데이터} 형식으로 설정한다.
- 현재 페이지에서 이동할 페이지로 전달하고 싶은 데이터를 [[객체(Object)]]로 전달할 수 있다.

- 전달한 데이터는 [[useLocation()]] 의 [[state]] [[속성(Property)]]로 받을 수 있다.

## 예시


```jsx
import { useNavigate } from "react-router-dom";

function CoursePage() {
	const navigate = useNavigate();
	
	// 클릭하면 위시리스트에 담고 위시리스트(/wishlist)로 페이지를 이동하는 함수 
	const handleAddWishlistClick = () => {
		addWishlist(course?.slug); 
	    navigate("/wishlist");
	};
	
	return  // 내용 생략
}
```

- 아래 코드에서 클릭하면 위시리스트에 코스가 담기고  위시리스트(/wishlist)로 페이지를 이동하며, 위시리스트에는 course 데이터가 전달되었다.

```jsx
// CoursePage.js
import { useNavigate } from "react-router-dom";

function CoursePage( { course } ) {
	const navigate = useNavigate();
	
	const handleAddWishlistClick = () => {
		addWishlist(course?.slug);
		navigate("/wishlist", {
			state: {
			    ...course,
		    },
		});
	};
	
	return  // 내용 생략
}
```

- [[useLocation()]]이 리턴하는 [[객체(Object)]]의 [[state]] [[속성(Property)]]에서 그 값을 확인할 수 있다.  

```jsx
import { Link, useLocation } from "react-router-dom";

function WishlistPage() { // /wishlist 부분
	const location = useLocation();
	console.log(location.state);
  
  //... 이하 생략
}
```


![](https://velog.velcdn.com/images/iberis/post/8a13e616-52a3-4d3e-a3e2-34c6fd2406cd/image.png)

- 참고로 [[<Link>]]태그 에서도 [[state]]를 보낼 수 있다.

```jsx
<Link to="/main" state={{ test: "hello world" }} >
	test
</Link>
```