- [[React Router]] v6에서 추가된 index routes는 특정 경로에 대한 기본 경로(default route)를 설정하는 방법이다.
- [[SPA(Single Page Application)]]의 특성상 배포시에 한번만 로딩을 하기 때문에 접속 엔드포인트가 명확하기 않으면 404오류가 뜰 수 있기 때문에 index routes 설정을 통해 이를 방지할 수 있다.

- 즉, 특정 경로로 접근할 때 경로가 정확히 일치하지 않을 때 기본 경로를 지정할 수 있다.
- 따라서 실제 배포시에 접속 url 엔드포인트 지정에 용이하다.

- 이전 버전인 v5에서는 기본 경로를 설정하기 위해서는 `<Redirect>` 컴포넌트를 사용해야 했다.
- index routes를 사용하면 `<Redirect>` 컴포넌트를 사용하지 않고도 경로에 대한 기본 경로를 설정할 수 있다.

- 또한, 중첩된 라우트에서도 일관성 있는 방식으로 기본 경로를 설정할 수 있어서 더욱 직관적인 코드를 작성할 수 있게 된다.
- index routes는 [[<Route>]] 컴포넌트에 index prop을 설정하여 구현할 수 있게 된다.


## 예시

- 예를 들어, 다음과 같이 /teams 경로에 대한 index route를 설정할 수 있다.

```jsx
<Routes>
	<Route path="/" element={<Home />} />
	<Route path="dashboard" element={<Dashboard />} />
</Routes>
```

- 여기서 /경로는 `<Home/>` 컴포넌트로, /dashboard 경로는 `<Dashboard/>` 컴포넌트로 매핑되는데 이 때, 사용자가 /dashboard 경로 이외의 다른 경로로 접근할 경우, 404 에러 페이지를 보여주는 것이 일반적이었다.

- 하지만 index routes를 사용하면, 다음과 같이 설정할 수 있다.

```jsx
<Routes>
	<Route path="/" element={<Home />} />
	<Route index element={<Dashboard />} path="dashboard" />
</Routes>
```

- 여기서 index속성을 추가하여 /dashboard 경로에 대한 기본 경로를 설정하게 된다.

- 이제 사용자가 /dashboard 경로 이외의 다른 경로로 접근하면, 여전히 404 에러 페이지를 볼 수 있지만, /dashboard 경로로 접근할 경우에는 `<Dashboard/>` 컴포넌트가 표시된다.

- index routes를 사용하면, 일치하는 경로가 없는 경우에 기본 페이지를 표시할 수 있으며, 코드의 가독성과 유지 보수성도 향상된다.

다른 예시로, 아래 코드에서 `<Route index>`는 /teams 경로에 대한 기본 경로를 설정하고 있다.  

```jsx
Route path="teams" element={<Teams />}>
  <Route path=":teamId" element={<Team />} />
  <Route path="new" element={<NewTeamForm />} />
  <Route index element={<LeagueStandings />} />
</Route>
```

- 이 경로에서 /teams와 정확히 일치하는 경로를 나태는데, index routes는 [[중첩 라우팅]]에서도 사용할 수 있다. 

- 예를 들어, 다음과 같이 /teams/:teamId 경로의 index route를 설정할 수 있다.

```jsx
<Route path="teams" element={<Teams />}>
	<Route path=":teamId" element={<Team />} >
		<Route index element={<TeamDashboard />} />
	</Route>
</Route>
```

- 위 코드에서 `<Route index>`는 /teams/:teamId 경로에 대한 기본 경로를 설정하고 있다.
- 이 경로는 /teams/:teamId와 정확히 일치하는 경로를 나타낸다.
