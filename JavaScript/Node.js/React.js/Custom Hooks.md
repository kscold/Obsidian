- [[컴포넌트(Component)]]를 만들다보면, 반복되는 로직이 자주 발생한다.

- 보통 src 디렉터리에 hooks 라는 디렉터리를 만들고 사용한다.
- 커스텀 Hooks 를 만들 때에는 보통 이렇게 `use` 라는 [[키워드(Keyword)]]로 시작하는 파일을 만들고 그 안에 [[함수(Function)]]를 작성한다.

- Custom Hooks를 만드는 방법은 [[함수(Function)]] 안에서 [[useState()]], [[useEffect()]], [[useReducer()]], [[useCallback()]] 등 [[Hooks]]를 사용하여 원하는 기능을 구현해주고, [[컴포넌트(Component)]]에서 사용하고 싶은 [[값(value)]]들을 반환해주면 된다.