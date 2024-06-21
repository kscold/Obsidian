- `<img>` 태그는 HTML 문서에서 이미지(image)를 정의할 때 사용한다.
- `<img>` 요소로 정의된 이미지는 [[HTML(Hyper Text Markup Language)]] 문서에 직접 삽입되는 것이 아니라, HTML 문서에 이미지가 링크되는 형태이다.

- 즉, <img> 요소는 이렇게 참조된 이미지를 위한 일종의 수용 공간을 만들어 주는 것이다.
- `<img>` 요소에는 src 속성과 alt 속성을 반드시 명시해야만 한다.

## alt 속성

- img 태그의 alt 속성은 src에 있는 이미지 경로가 없거나 늦게 이미지가 불러와질 때 보여지는 텍스트이다. 
- 브라우저에 접속했을 때 가끔 정상적으로 뜨지 않고 글씨만 뜨는 경우 보여지는 텍스트를 의미한다.

```jsx
<img src="exercise.png" alt="운동"/>
```


## HTML에서의 예시

```html
<img src="/examples/images/img_monalisa.png" alt="모나리자">
```

## 리액트 컴포넌트에 이미지를 추가하는 방법

- [[리액트(React)]]에는 빌드 단계에 이미지를 포함할 방법이 필요하다.
- 주로 사용하는 2가지 방법이 있습니다.

- Create React App 프로젝트를 가정한다.
- public 디렉토리의 모든 것이 서버로 복사된다.
- src 아래의 모든 것이 JS 파일로 가져오는 것이 타당한 파일들이다.

### 1 .이미지를 컴포넌트로 임포트하기

- 이미지 파일을 src 폴더 아래 어딘가에 넣는다.
- 이것만으로는 바로 사용할 수 없으므로 이미지를 사용 중인 React [[컴포넌트(Component)]]로 가져온다.

```jsx
import companyLogo from './path/to/logo.jpg';
```

- 다음에 해당 변수 이름으로 해당 이미지를 참조할 수 있다.
- 이름은 원하는 대로 지정할 수 있으며 이미지 파일명와 일치하지 않아도 된다.
- 이미지를 표시하려는 곳마다 img 태그를 렌더링하고 해당 변수를 src로 전달한다.

```jsx
function Home() {
	return (
		<div>
			<img src={companyLogo} alt="BigCo Inc. logo"/>
		</div>
	);
}
```

- 가져온 이미지를 사용하는 경우 중괄호를 사용해야 한다.
- 중괄호는 JS 변수를 프롭으로 전달하는 방법이다.

### 2.이미지를 퍼블릭 폴더에 놓기

- 이미지 파일을 퍼블릭 폴더에 넣을 수 있다.(Create React App이 아닌 경우, 서버에 복사될 폴더에 놓을 수 있다.)

- 그런 다음 서버가 퍼블릭 폴더를 "root" 디렉토리(/)로 취급한다고 가정하면 일반 [[HTML(Hyper Text Markup Language)]]과 마찬가지로 해당 디렉토리에 대해 이미지를 사용할 수 있다.
- 따라서 public/images/thing.jpg에 이미지가 있는 경우 해당 이미지를 다음과 같이 표시할 수 있다.

```jsx
function Home() {
	return (
		<div>
			<img src="images/logo.jpg" alt="BigCo Inc. logo"/>
		</div>
	);
}
```

- 이 방법을 사용하면 웹 서버에서 이미지를 일반 파일로 사용할 수 있고 브라우저에서 `http://localhost:3000/images/logo.jpg`(또는 실제 도메인 이름, 배포되었을 때) 경로로 해당 파일을 오픈할 수 있다.

## 리액트에서 임포트한 이미지의 동작

- [[import]]는 React가 처리하지 않는다.
- 그것들은 [[웹팩(webpack)]]/Vite일 번들러에 의해 처리된다.(Create React App을 사용하는 경우 확실히 Webpack이다.)

- Webpack, Rollup, Parcel 및 기타 번들러는 모두 개념적으로 동일한 방식으로 동작한다.
- 이미지나 CSS 파일과 같은 정적 파일을 가져올 때 번들러는 해당 파일을 그대로 export 위치에 복붙하지 않는다.
- 대신 이 특정 JS 파일이 이 특정 이미지/CSS 파일/무엇이든 어딘가에 의존한다는 점을 기록한다.

- 그런 다음 번들러는 생성된 고유 이름(예: a5c8d3f89cad.jpg)으로 이미지를 output 디렉토리에 복사하고,

```jsx
<img src={yourName}/> // <img src="a5c8d3f89cad.jpg" />로 대체
```

- 이미지가 특히 작은 경우 Webpack은 최적화를 위해 이를 JS 번들에 인라인하기로 결정할 수도 있다.
- 이 모든 것을 개발자가 걱정할 필요는 없다.

## 리액트에서 img 태그를 사용하는 최고의 방법

- 컴포넌트와 관련된 일회성 이미지의 경우 [[import]]를 권장한다.
- 가져온 이미지 파일이 없으면 빌드가 실패하고 오류를 빠르게 찾을 수 있다는 부수적인 이점이 있다.