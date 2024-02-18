- describe()는 [[Jest]] 테스트를 그룹화하는 즉, [[테스트]] 단위를 묶는 가장 큰 단위이다.
- describe에 설명을 적어 테스트 단위들을 하나로 묶어서 관리할 수 있게 해주는 역할을 한다.
- 심지어 describe는 중첩이 가능해서 테스트를 자유롭게 그룹화해서 관리할 수 있다.

- 이는 테스트가 어떤 모듈이나 기능에 관련되어 있는지 명확하게 표현하는 데 도움이 된다.

```jsx
// describe 블록 사용 예
describe('String 관련 테스트', () => {
	test('문자열 합치기', () => {
		expect('Hello' + 'World').toBe('HelloWorld');
	});
	
	it('문자열 길이', () => {
		expect('Hello'.length).toEqual(5);
	});
});
```