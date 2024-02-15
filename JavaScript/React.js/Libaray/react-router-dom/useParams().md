- [[URL 파라미터]]는 useParams라는 [[Hooks]]을 사용하여 [[객체(Object)]] 형식으로 조회할 수 있다.
- [[URL 파라미터]]의 이름은 라우트 설정을 할 때 Route [[컴포넌트(component)]]의 path [[props]]를 통해 설정된다.

## 예시

- 상위 [[컴포넌트(component)]]에서 [[URL 파라미터]]를 /:이름 형식으로 정의한다.

```jsx
import React from 'react';
import { Route, Routes } from 'react-router-dom';
import About from './page/About';
import Home from './page/Home';
import Profile from './page/Profile';

  
const App = () => {
	return (
		<Routes>
			<Route path="/" element={<Home />} />
			<Route path="/about" element={<About />} />
			<Route path="/profiles/:username" element={<Profile />} />
		</Routes>
	);
};

export default App;
```

- [[useParams()]]의 [[인스턴스(Instance)]]를 생성하고, 인스턴스를 통해 [[객체(Object)]][[속성(Property)]]으로 접근할 수 있다.
- 밑에 [[URL 파라미터]]로 들어온 값이 있다면 [[조건부 렌더링]]을 하는 예시이다.

```jsx
import React from 'react';
import { useParams } from 'react-router-dom';


const data = { // 임의의 객체 정의
	velopert: {
		name: '김민준',
		description: '리액트를 좋아하는 개발자',
	},
	gildong: {
		name: '홍길동',
		description: '고전 소설 홍길동전의 주인공',
	},
};
  
const Profile = () => {
	const params = useParams(); // 인스턴스 생성
	const Profile = data[params.username]; // 클라이언트가 들어간 URL 파라미터 이름 검색
	
	return (
		<div>
			<h1>사용자 프로필</h1>
			{Profile ? ( // 프로필이 있으면 조건부 렌더링
				<div>
					<h2>{Profile.name}</h2>
					<p>{Profile.description}</p>
				</div>
				) : (
				<p>존재하지 않는 프로필입니다.</p>
			)}
		</div>
	);
};

export default Profile;
```