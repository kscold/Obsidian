-  [[생성자(Constructor)]] [[함수(Function)]] 만들거나 [[클래스(class)]]로 선언된 [[함수(Function)]]의 원형을 찍어내듯 사용하는데 이렇게 생성된 [[객체(Object)]]를 인스턴스라고 한다.


## 예시

- [[생성자(Constructor)]] = 거푸집  
- 인스턴스 = 거푸집으로 찍어낸 칼

- 밑에 코드는 칼을 만드는 작업을 현실세계에 비교한 예시이다.

```js
function Sword(color, metal) {
  this.color = color;
  this.metal = metal;
  this.is = function() {
    console.log(`This is ${this.color} ${this.metal} sword!`);
  };
}
const redSteel = new Sword('red', 'steel');

console.log(redSteel); //Sword {color: 'red', metal: 'steel', is: ƒ}

redSteel.is(); //This is red steel sword!
```