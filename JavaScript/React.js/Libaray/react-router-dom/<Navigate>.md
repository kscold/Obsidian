- [[React Router]] 라이브러이의 기능으로 특정 경로에서 렌더링 시점에 다른 페이지로 이동시키고 싶을 때 사용한다.

- `<Navigate to="경로">` 는 렌더링될 때 현재 위치를 변경한다.  

- `<Route>` 에 지정한 경로 외의 경로로 접속했을 때 보여줄 [[컴포넌트(component)]]를 지정할 때에는 path= 로 지정해주면 된다.

## 문법

```jsx
<Navigate to="이동시키려는 경로" />;
```


```jsx
import { useParams, Navigate } from "react-router-dom";


function CoursePage() {
	const { courseSlug } = useParams(); // 현재 경로 리턴
	const course = getCourseBySlug(courseSlug); 
	// courseSlug 값에 따라 course 데이터 객체를 리턴 함수
	
	const courseColor = getCourseColor(course?.code); // 옵셔널 사용(code가 있으면)
	
	// course 값이 없다면 /courses 경로로 이동 
	if (!course) {
		return <Navigate to="/courses" />;
	}
	
	// 내용 생략 
```

- 위의 코드에서 하위 경로인 `/courses` 의 아래에 잘못된 경로(`/courses/wrongPath` )로 접속하면 빈 화면이 보여지게 된다.
- 이 때 잘 못된 경로에 대해 다른 경로로 이동시키도록 Navigate를 사용할 수 있다.