- addEventListener()는 document의 특정요소([[id]], [[class]], tag 등) [[이벤트(event)]]( 예) click하면 함수를 실행한다, 마우스를 올리면 함수를 실행한다)를 등록할 때 사용한다.

## 문법

```js
.addEventListener(eventType, eventHandler, options);
```

- eventType: 이벤트 유형을 나타내는 문자열이다.( 예) 'click', 'scroll' 등이 있다.)
- eventHandler: 이벤트가 발생했을 때 실행될 [[함수(Function)]] 또는 [[콜백 함수(Callback Function)]]를 전달한다.
- options: 이벤트 리스너 등록에 대한 추가적인 옵션을 설정할 수 있습니다. 객체 또는 불리언 값을 사용할 수 있ek.

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