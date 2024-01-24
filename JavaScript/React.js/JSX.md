- JSX는 자바스크립트의 확장 문법이며 XML과 매우 비슷한 형태를 가진다.

- JSX는 리액트로 프로젝트를 개발할 때 사용되므로 공식적인 자바스크립트 문법은 아니며, 개발자들이 babel에서 여러 문법을 지원할 수 있도록 preset 및 plugin을 설정한 것이다.

- 브라우저에 실행되기 전에 코드가 번들링되는 과정에서 바벨을 사용하여 일반 자바스크립트 형태의 코드로 변환된다.

```jsx
funtion App() {
	return (
	<div>
		Hello <b>react</b>
	</div>
	)
}
```

위의 JSX 문법은 밑에 코드로 변환된다.

```js
funtion App(){
	return React.createElement("div", null, "Hello ", React.createElement("b", null, "react"))
}
```