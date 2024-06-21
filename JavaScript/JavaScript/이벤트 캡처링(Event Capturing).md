- 캡처링은 [[이벤트 버블링(Event Bubbling)]]과 반대로 최상위 태그에서 해당 태그를 찾아 내려간다.

- 캡처링은 잘 사용되지는 않는다.


## 이벤트 캡쳐링의 예시

![](https://joshua1988.github.io/images/posts/web/javascript/event/event-capture.png)


```js
const divs = document.querySelectorAll("div");

const clickEvent = (e) => {
	console.log(e.currentTarget.className);
};

divs.forEach((div) => {
	div.addEventListener("click", clickEvent, { capture: true });
});
```

![](https://images.velog.io/images/tlatjdgh3778/post/701c6e40-8294-4c99-9b2b-2a0e0ad39b5f/image.png)

- [[addEventListener()]] 의 옵션 [[객체(Object)]]에 `{ capture: true }` 또는 `true` 를 설정해주면 캡처링을 구현할 수 있다.

- `<div class="DIV3">DIV3</div>`를 클릭한다면 위에서부터 찾아 내려오기 때문에 콘솔에는 DIV1, DIV2, DIV3이 순서대로 찍힐 것이다.

![](https://images.velog.io/images/tlatjdgh3778/post/49fe0a56-f915-4188-8634-bc050ee6c1ac/image.png)