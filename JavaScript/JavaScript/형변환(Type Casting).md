- 자바스크립트 [[변수(Variable)]]는 새 [[변수(Variable)]] 및 다른 데이터 유형으로 변환할 수 있다.

- 자바스크립트 [[함수(Function)]]를 사용해서 변환할 수 있고 혹은 자바스크립트 자체에 의해 자동으로 변환할 수 있다.


## 함수 이용 타입 변환

- 대표적인 [[클래스(Class)]]인 String()과 Number()를 사용하여 변환시킬 수 있다.
- String 타입일 때는 toString() [[메서드(Method)]]와 문자의 길이를 측정하는 length를 사용할 수 있다.

```js
let val;

// Number to String
val = String(111);
val = String(8 + 4);

// Boolean to String
val = String(false);

// Date to String
val = String(new Date());

// Array to String
val = String([1, 2, 3, 4, 5]);

// toString()
val = (5).toString();

console.log(val.length); // String일때는 length를 사용하여 길이를 측정할 수 있음

// String to number
val = Number('1'); // 1
val = Number(true); // 1
val = Number(false); // 0
val = Number(null); // 0
val = Number([1, 2, 3]); // 배열의 경우 NaN(Not a Number)으로 변환됨

val = parseInt('111.40');
val = parseFloat('111.40');

console.log(val);

console.log(typeof val);
```


## 자바스크립트가 자동적으로 타입을 변경

- 아래 예시의 경우 자동적으로 자바스크립트가 val2의 타입을 변경하여 typeof를 사용해보면 23이 나오게 된다.

```js
const val1 = String(2);
const val2 = 3;
const sum = val1 + val2; // 자동적으로 자바스크립트가 val2의 타입을 변경함

  
console.log(sum);
console.log(typeof sum);
// >> 23
// >> String
```