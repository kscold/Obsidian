- [[querySelector()]]와 사용 방법은 동일하며 선택자를 선택하여 [[배열(Array)]]과 비슷한 [[객체(Object)]]인 nodeList를 반환한다.

- 반환객체가 nodeList이기에 for문 또는 forEach문을 사용한다.

- 아래 코드와 같이 ","를 사용하면 여러 요소를 한번에 가져올 수 있다.

## 문법

```js
querySelectorAll("#id, .class");
```

## 예시

```html
<div id="sections">
	<ol class="section">
		1
		<li>1-1</li>
	    <li>1-2</li>
	    <li>1-3</li>
	</ol>
	
	<ol class="section">
	    2
	    <li>2-1</li>
	    <li>2-2</li>
	    <li>2-3</li>
	</ol>
</div>
```

```js
const sections = document.querySelectorAll("#sections , #sections .section");
// id가 sections인 태그와 class가 section인 경우를 모두 선택

console.log(sections.constructor.name); // NodeList를 반환

for(const i = 0; i < sections.length; i++) {
	const item = sections.item(i);
	item.style.border = "1px solid #ff0000";
}
```