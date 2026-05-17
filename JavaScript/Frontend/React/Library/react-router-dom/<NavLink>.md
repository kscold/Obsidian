- `<NavLink>` [[컴포넌트(Component)]]는 링크에서 사용하는 현재 라우트 경로와 일치하는 경우 특정 스타일 또는 CSS 클래스를 적용하는 [[컴포넌트(Component)]]이다.


## 문법

- 밑의 코드는 `<NavLink>` [[컴포넌트(Component)]]의 [[props]]
```jsx
type NavLinkRenderProps = {
	isActive: boolean;
	isPending: boolean;
	isTransitioning: boolean;
};
```
- `<NavLink>` [[컴포넌트(Component)]]의 style과 className [[props]]는 { isActive: boolean }을 [[매개변수(parameter)]]로 전달받는 [[함수(Function)]] 타입의 값을 전달한다.

## 예시

```jsx
<NavLink
	to="/articles/1" // 목표 경로 설정
	style={({isActive}) => isActive ? activeStyle : undefined} // 목표 경로로 이동했으면
/>

```