- `<Suspense>`는 [[비동기(asynchronous)]]로 데이터 가져오기와 코드 스플리팅을 더욱 간단하게 처리하고, 사용자 경험을 개선하기 위한 목적으로 도입된 기능이다. 
- `<Suspense>`는  v16.6 이상부터 사용 가능하며, 다음과 같은 주요 목적을 가지고 있습니다.

## [[비동기(asynchronous)]] 데이터 가져오기

- Suspense를 사용하면 [[컴포넌트(Component)]]에서 비동기 작업을 수행하거나 외부 데이터를 가져오는 동안 화면에 로딩 상태를 표시할 수 있다.
- 이로써 사용자 경험을 향상시킬 수 있다.

## 코드 스플리팅

- Suspense는 코드 스플리팅과 함께 사용하여 앱 번들의 크기를 줄이고 초기 로딩시간을 최적화할 수 있다.
- 필요한 컴포넌트만 로드하고 사용자가 해당 컴포넌트를 요청할 때까지 다른 컴포넌트를 로드하지 않도록 한다.

```jsx
<Suspense fallback={<div>Loading...</div>}>
	<Routes>
		<Route path="/" element=<About/>} />
	</Routes>
</Suspense>
```

## fallback

- fallback [[props]]은 [[컴포넌트(Component)]]가 로드될 때까지 기다리는 동안 렌더링하려는 React 엘리먼트를 받는다.

- [[<Suspense>]] 컴포넌트는 lazy 컴포넌트를 감싼다.
- 하나의 Suspense 컴포넌트로 여러 [[lazy()]] [[컴포넌트(Component)]]를 감쌀 수도 있다.
- [[lazy()]] [[컴포넌트(Component)]]는 Suspense 컴포넌트 하위에서 렌더링되어야 하며, Suspense는 lazy 컴포넌트가 로드되길 기다리는 동안 로딩 화면과 같은 예비 콘텐츠를 보여줄 수 있게 해 준다.