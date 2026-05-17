- 사용자가 직접 [[이벤트(event)]]를 누르는 것 대신 [[Jest]]를 이용하여 이벤트를 일으킬 때 사용하는 메서드이다.
- [[JSX]]에 [[HTML(Hyper Text Markup Language)]] 속성인 [[role]]를 통해 문자열로 이름을 지정하고 이를 [[screen]]으로 요소를 가져와서 fireEvent로 [[이벤트(event)]]를 발생시킨다.

## click()

```js
it("버튼을 눌렀을 때, 제대로 작동하는지 테스트하자!", () => {
	render(<JestUnitTestPage />);
	
	screen.getByRole("count-button");
	fireEvent.click(screen.getByRole("count-button"));
	expect(screen.getByRole("count")).toHaveTextContent("1");

});
```

## change()

```jsx
it("게시글이 잘 등록되는지 테스트 하자!", () => {
	render(<StaticRoutingMovedPage />);
	
	fireEvent.change(screen.getByRole("input-writer"), {
		target: { value: "맹구" },
	});
	expect(screen.getByRole("count")).toHaveTextContent("1");
});
```