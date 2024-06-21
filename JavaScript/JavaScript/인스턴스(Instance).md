- [[생성자 함수(Constructor Function)]]를 만들거나 [[클래스(Class)]]로 선언된 [[함수(Function)]]의 원형을 찍어내듯 사용하는데 이렇게 생성된 [[객체(Object)]]를 인스턴스라고 한다.

## 예시

- [[생성자(Constructor)]]는 거푸집이다.
- 인스턴스는 거푸집으로 찍어낸 칼이다.

- 따라서 밑에 코드는 칼을 만드는 작업을 현실세계에 비교한 예시이다.

```js
function Sword(color, metal) {
	this.color = color;
	this.metal = metal;
	
	this.is = function() { // 메서드(객체 속성에 정의된 함수)
		console.log(`This is ${this.color} ${this.metal} sword!`);
	};
}
const redSteel = new Sword('red', 'steel');

console.log(redSteel); // Sword {color: 'red', metal: 'steel', is: ƒ}

redSteel.is(); // This is red steel sword!
```