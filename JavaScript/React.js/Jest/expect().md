- expect는 값을 테스트할 때마다 사용된다.

- `toXxx` 부분에서 사용되는 함수를 흔히 Test Matcher라고 하는데요. 위에서 사용된 `toBe()` 함수는 숫자나 문자와 같은 [[객체(Object)]]가 아닌 [[원시 타입(Primitive type)]](기본형(primitive)) 값을 비교할 때 사용된다.
- 따라서 expect는 혼자서 사용하는 게 아니라 expect() 안에 테스트할 [[변수(Variable)]]나 [[값(value)]]을 넣고 matcher 함수들 즉, toBe/toEqual 같은 것들과 같이 사용된다.

```js
it("더하기 잘 되는지 테스트 해보기", () => { // it으로 테스트
	const result = add(3, 5); // add라는 함수를 불러온 것임
	expect(result).toBe(8); // expect에는 변수를 설정하고 .toBe 메서드를 통해 결과 예측
});
```

## Test Matcher
### toBe, [[toEqual()]]

- toBe는 기본 값을 단순 비교하거나, 객체 [[인스턴스(Instance)]]의 참조 ID를 확인하는데 사용된다.
- 공식문서에서는 부동 소수점 숫자와는 함께 사용하지 말라고하며 대신 다른 것(toBeCloseTo)을 이용하라고 나와있다.

- toEqual은 [[배열(Array)]]이나 [[객체(Object)]] 내부까지 깊은 비교를 진행해준다.

- 하지만 두 객체 인스턴스의 내부 값들만 비교해주는 것이지 두 참조 ID가 동일한지를 비교하는 것은 아니다.

### beforeEach, afterEach

- beforeEach는 각 테스트가 실행되기 전에 실행되는 전처리기이다.
- 위 예제를 보면 각 test() 와 [[it()]]이 실행하기 전에 beforeEach가 실행되며 num이 1로 할당되는 것이다.
- afterEach는 각 테스트가 실행된 후에 실행되는 후처리기이다.
- 위 예제를 보면 각 test()와 it()이 실행한 후에는 num을 0으로 할당한다.
- 위 예제에서는 쓸모가 없어 보이지만 일부 임시 상태를 정리하는데 유용하게 사용된다.

### toBeInTheDocument()

- [[Jest]]의 [[Virtual DOM]] 안에 잘 렌더링 되는지 확인할 때 사용하는 메서드이다.
- 사용하기 위해서는 @testing-library/jest-dom와 "@testing-library/react"의 { render, screen }를 임포트하여 사용한다.

### toMatchSnapshot()

- 일전 렌더링되는 [[컴포넌트(component)]]의 사진을 찍어 비교하는 메서드이다.
- 사진이 없는 경우에는 사진을 찍게 되고 사진이 있는 경우는 비교하게 된다.
- yarn test를 하게 되면 컴포넌트의 렌더링을 찍어 `__snapshot__`이라는 폴더에 코드 형태가 생긴다.
- 사진을 다시 찍고 싶다면 yarn test -u를 사용하여 update 하면 된다.

## toHaveTextContent()

- [[이벤트(event)]]를 통해 렌더링 변경되는 영역 내용의 예상 값을 적용하는 메서드이다.