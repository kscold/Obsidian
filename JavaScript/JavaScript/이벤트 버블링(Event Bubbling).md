- 이벤트 버블링이란 한 요소(Element)에 [[이벤트(event)]]가 발생하면 이 요소에 할당된 핸들러가 동작하고, 이어서 부모 요소의 핸들러가 동작하고 최상단의 부모 요소를 만날 때까지 반복되면서 핸들러가 동작하는 현상을 말한다.

- [[이벤트 캡처링(Event Capturing)]]과 반대되는 개념이다.


## 이벤트의 과정

- 최상위 [[window]] 객체에서부터 [[이벤트(event)]]가 내려오는 [[이벤트 캡처링(Event Capturing)]] 단계를 통해 [[이벤트(event)]]가 하위 요소로 전파된다.
- 이후 [[이벤트(event)]]가 실제 타겟(target)에 요소로 전달되는 단계를 거쳐 [[이벤트(event)]]가 상위 요소로 전파되는 이벤트 버블링이 일어나게 된다.

- 따라서 즉, [[이벤트 캡처링(Event Capturing)]] 단계, 타겟 단계, 이벤트 버블링 단계로 이루어진다고 생각하면 된다.

![[Pasted image 20240501134435.png]]


## 이벤트 버블링의 예시

![](https://joshua1988.github.io/images/posts/web/javascript/event/event-bubble.png)


```html
 <body>
    <div class="DIV1">
	    DIV1
	    <div class="DIV2">
		    DIV2
		    <div class="DIV3">DIV3</div>
	    </div>
	</div>
</body>
```

```js
const divs = document.querySelectorAll("div");

const clickEvent = (e) => {
	console.log(e.currentTarget.className);
};

divs.forEach((div) => {
	div.addEventListener("click", clickEvent);
});
```

![](https://images.velog.io/images/tlatjdgh3778/post/701c6e40-8294-4c99-9b2b-2a0e0ad39b5f/image.png)

- div를 클릭하면 해당하는 클래스([[class]])의 이름이 콘솔로 출력되는 코드이다.
- 자바스크립트는 기본적으로 버블링이 발생하기 때문에 `<div class="DIV3">DIV3</div>`를 클릭한다면 콘솔에는 DIV3, DIV2, DIV1이 순서대로 출력이 될 것이다.

![](https://images.velog.io/images/tlatjdgh3778/post/10c06185-a1ac-435b-9f53-7dea1cdf6b60/image.png)

- `<div class="DIV3">DIV3</div>` 를 클릭하면 할당되어 있는 [[이벤트(event)]]가 발생하고 다음에는 바깥의 [[<div>]] 태그에 할당된 [[이벤트(event)]]가 발생하고 [[document]] [[객체(Object)]]를 만날 때까지 [[이벤트(event)]]가 전파된다.


## 이벤트 버블링(전파)을 막는 방법

- 이벤트 버블링은 해당 타깃에서 [[document]] [[객체(Object)]]를 만날 때까지 핸들러가 모두 호출되는데 코드를 작성하다보면 원하는 타깃에서만 [[이벤트(event)]]를 발생하게 하고 싶을때가 있다.

- 그럴 때에는 `event.stopPropagation()` 을 사용하면 되는데 버블링의 경우에는 클릭한 타깃의 [[이벤트(event)]]만 발생하고 상위 요소로 [[이벤트(event)]]가 전파되는 것을 막을 수 있다.

- 버블링에서 작성된 코드에 `event.stopPropagation()` 을 추가하고 실행해보자.

```js
const clickEvent = (e) => {
	e.stopPropagation();
	console.log(e.currentTarget.className);
};
```

- 아까는 DIV3을 클릭했을 때 이벤트가 전파되어 DIV3, DIV2, DIV1 이 출력이 됐지만 이번에는 클릭한 타깃의 이벤트만 발생하는 것을 알 수 있다.

![](https://images.velog.io/images/tlatjdgh3778/post/5d6023bb-86e4-45bd-9829-caeb4448e416/image.png)