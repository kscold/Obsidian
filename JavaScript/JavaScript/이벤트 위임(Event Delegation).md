- [[이벤트 버블링(Event Bubbling)]]과 [[이벤트 캡처링(Event Capturing)]]을 통해서 [[이벤트(event)]] 위임을 할 수 있다.

- 이벤트 위임은 하위 요소(Element)의 [[이벤트(event)]]를 상위 요소에 위임하는 것이다.


## 이벤트 위임의 예시

- 아래는 이벤트 위임이 들어가 있지 않은 코드의 예시이다.

```js
const buttons = document.getElementsByClassName('buttonClass');

for (const button of buttons) {
	button.addEventListener('click', () => {
		alert('clicked');
	});
}

const buttonList = document.querySelector('#buttons');
const button = document.createElement('button');

button.setAttribute('class', 'buttonClass');
button.innerText = 'Click me';
buttonList.appendChild(button);  
```

- 아래는 이벤트 위임이 들어가 있는 코드이다.

```js
const buttonList = document.querySelector('#buttons');

buttons.addEventListener('click', () => {
	alert('clicked');
});

const button = document.createElement('button');

button.setAttribute('class', 'buttonClass');
button.innerText = 'Click me';
buttonList.appendChild(button);
```