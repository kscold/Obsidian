- addEventListener()는 [[document]]의 특정요소([[id]], [[class]], tag 등) [[이벤트(event)]]를 등록할 때 사용한다.

- 예를 들어 click하면 [[함수(Function)]]를 실행한다, 마우스를 올리면 [[함수(Function)]]를 실행하는 동작 등이 있다.


## 문법

```js
.addEventListener(eventType, eventHandler, options);
```

### eventType

- 이벤트 유형을 나타내는 문자열이다.

| 이벤트 종류     | 이벤트 동작                                      |
| ---------- | ------------------------------------------- |
| auxclick   | 요소를 non-primary button으로 클릭할 때 발생한다.        |
| click      | 요소를 primary button으로 클릭할 때 발생한다.            |
| dblclick   | 요소를 primary button으로 더블 클릭할 때 발생한다.         |
| mousedown  | 요소 위로 마우스를 누를 때 발생한다.                       |
| mouseup    | 요소 위에 있는 마우스를 놓을 때 발생한다.                    |
| mouseenter | 포인터가 요소 위로 이동될 때 발생한다.<br>[[이벤트 버블링(Event Bubbling)]]이 없다.  |
| mouseleave | 포인터가 요소 밖으로 이동될 때 발생한다.<br>[[이벤트 버블링(Event Bubbling)]]이 없다. |
| mousemove  | 포인터가 요소 위에 있으면서 이동 중일 때 발생한다.               |
| mouseover  | 포인터가 요소 안(하위 요소에서 오는 경우 포함)으로 이동될 때 발생한다.   |
| mouseout   | 포인터가 요소 밖(하위 요소로 가는 경우 포함)으로 이동될 때 발생한다.    |
| keypress   | 키를 눌렀을 때 동작한다.                              |
| focus      | 누른 태그가 포커스 된다.                              |
| blur       | 포커스가 되었다가 밖으로 나가면 동작하게 된다.                  |
| cut        | 잘나내기할 때 동작한다.                               |
| paste      | 붙여넣기할 때 동작한다.                               |
| input      | 요소를 입력할 때 동작한다.                             |
### eventHandler

- [[이벤트(event)]]가 발생했을 때 실행될 [[함수(Function)]] 또는 [[콜백 함수(Callback Function)]]를 전달한다.
### options

- 이벤트 리스너 등록에 대한 추가적인 옵션을 설정할 수 있다.
- [[객체(Object)]] 또는 불리언(Boolean) 값을 사용할 수 있다.


## 예시

```html
<div id="cols">
	<button class="btn">A</button>
	<button class="btn">B</button>
	<button class="btn">C</button>
</div>
```

```js
var cols = document.querySelectorAll("#cols .btn"); // id cols의 요소 btn을 모두 선택

for (var i = 0; i < cols.length; i++) {
	cols[i].addEventListener("click", click); // cols의 요소들에 클릭 이벤트를 추가
}

cols[1].style.color = "red";

function click(e) {
	window.alert(this.innerHTML);
}
```

- 위의 코드는 [[querySelectorAll()]] 함수를 이용해 div안의 id 값 'cols'와 button의 class값 'btn '값을 가져온 후, 반복문을 사용하여 해당하는 모든 값에 addEventListener를 지정해 클릭시 click함수를 실행하도록 하는 스크립트이다.
- 또한 1번 배열의 색을 "red"로 지정함으로 .btn 1번 값인 B가 붉은색으로 변화한다.

- 위와 같이 작성하면 버튼 추가시에도 똑같은 코드를 재작성 할 필요 없이 자동으로 이벤트가 등록된다.