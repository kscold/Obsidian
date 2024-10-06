- 결론부터 말하자면 , 깊은 복사는 객체의 실제 값([[원시 타입(Primitive type)]])을 복사한다.

- [[원시 타입(Primitive type)]]을 복사할 때 그 값은 또 다른 독립적인 메모리 공간에 할당하기 때문에, 복사를 하고 값을 수정해도 기존 [[원시 타입(Primitive type)]]을 저장한 [[변수(Variable)]]에는 영향을 끼치지 않는다.

## 변수와 객체에서의 복사의 차이

- 실제 값을 복사하는 것을 깊은 복사라고 한다.
- 하지만 이것은 자료형을 깊은 복사한 것이다.


```js
// 깊은 복사 예시
const a = 'a';
let b = 'b';
b = 'c';

console.log(a); // 'a';
console.log(b); // 'c'; 
// 기존 값에 영향을 끼치지 않음
```

 - [[참조 타입(Reference Type)]]을 복사할 때는 [[변수(Variable)]]가 [[객체(Object)]]의 참조를 가리키고 있기 때문에 복사된 변수 또한 객체가 저장된 메모리 공간의 참조를 가리키고 있다.

 - 그래서 복사를 하고 [[객체(Object)]]를 수정하면 두 변수는 똑같은 참조를 가리키고 있기 때문에 기존 객체를 저장한 변수에 영향을 끼친다.
 - 이처럼 객체의 [[참조 타입(Reference Type)]](주소값)을 복사하는 것을 [[얕은 복사]]라고 한다.

- 아래는 깊은 복사의 예시 코드이다.

```js
// 얕은 복사 예시
const a = { one : 1,  two : 2 };
let b = a; 
b.one = 3;

console.log(a); // { one: 3, two: 2 } 출력
console.log(b); // { one: 3, two: 2 } 출력 
// 기존 값에 영향을 끼침
```

- 일반적으로 복사라는 개념을 생각한다면 깊은 복사가 떠오를 것이다.
- 하지만 [[객체(Object)]]를 복사할 때  =  [[키워드(Keyword)]]를 사용해서 복사하면 [[얕은 복사]]가 돼서 기존 변수 또한 수정하게 된다.


## 깊은 복사 방법

- 깊은 복사된 객체는 객체 안에 객체가 있을 경우에도 원본과의 참조가 완전히 끊어진 객체를 말한다.

- 복사를 하는 목적은 기존 객체의 값만 복사본으로 가져와 별도로 활용하기 위함이 대부분이라고 생각한다.

### JSON.[[parse()]] && JSON.[[JSON.stringify()]] 

 - JSON.[[JSON.stringify()]]는 [[객체(Object)]]를 [[JSON(Java Script Object Notation)]] 문자열로 변환하는데 이 과정에서 원본 [[객체(Object)]]와의 참조가 모두 끊어진다.

- [[객체(Object)]]를 [[JSON(Java Script Object Notation)]] 문자열로 변환 후, JSON.[[parse()]]를 이용해 다시 원래 객체(자바스크립트 객체)로 만들어준다.

- 이 방법이 가장 간단하고 쉽지만 다른 방법에 비해 느리다는 것과 [[객체(Object)]]가 function일 경우,  undefined로 처리한다는 것이 단점입니다.

```js
const object = {  
	a: "a",  
	number: {    
		one: 1,    
		two: 2,  
	},  
	arr: [1, 2, [3, 4]]
	}; 

const copy = JSON.parse(JSON.stringify(object)); // 객체를 json변환하고 다시 json를 객체로 변환

copy.number.one = 3;
copy.arr[2].push(5);

console.log(object === copy); // false
console.log(object.number.one === copy.number.one); // false 깊은 복사를 했기 때문에
console.log(object.arr === copy.arr); // false
console.log(object); // { a: 'a', number: { one: 1, two: 2 }, arr: [ 1, 2, [ 3, 4 ] ] }
console.log(copy); // { a: 'a', number: { one: 3, two: 2 }, arr: [ 1, 2, [ 3, 4, 5 ] ] }
```

### 재귀 함수를 구현한 복사

 - 복잡하다는 것이 단점이다.

```js
const object = {  
	a: "a",  
	number: {    
		one: 1,    
		two: 2,  
	},  
	arr: [1, 2, [3, 4]]
};

function deepCopy(object) {  
	if (object === null || typeof object !== "object") { 
		return object;  
	}  // 객체인지 배열인지 판단
	
	const copy = Array.isArray(object) ? [] : {};   
	
	for (let key of Object.keys(object)) {    
		copy[key] = deepCopy(object[key]);  
	}   
	
	return copy;
}

const copy = deepCopy(object); 
copy.number.one = 3;
copy.arr[2].push(5);

console.log(object === copy); // false
console.log(object.number.one === copy.number.one); // false
console.log(object.arr === copy.arr); // false

console.log(object); // { a: 'a', number: { one: 1, two: 2 }, arr: [ 1, 2, [ 3, 4 ] ] }
console.log(copy); // { a: 'a', number: { one: 3, two: 2 }, arr: [ 1, 2, [ 3, 4, 5 ] ] }
```

### Lodash 라이브러리 사용

 - 라이브러리를 사용하면 더 쉽고 안전하게 깊은 복사를 할 수 있다.
 - 설치를 해야 한다는 점과 일반적인 개발에는 효율적이겠지만, 코딩 테스트에는 사용할 수 없다는 것이 단점이다.

```js
const deepCopy = require("lodash.clonedeep")
const object = {  
	a: "a",  
	number: {    
		one: 1,    
		two: 2,  
	},  
	arr: [1, 2, [3, 4]]
};

const copy = deepCopy(object); 
copy.number.one = 3;
copy.arr[2].push(5); 
console.log(object === copy); // false
console.log(object.number.one === copy.number.one); // false
console.log(object.arr === copy.arr); // false

console.log(object); // { a: 'a', number: { one: 1, two: 2 }, arr: [ 1, 2, [ 3, 4 ] ] }
console.log(copy); // { a: 'a', number: { one: 3, two: 2 }, arr: [ 1, 2, [ 3, 4, 5 ] ] }
```

### structuredClone() 메서드 사용

- 비교적 최근에 추가된 바닐라 자바스크립트 [[메서드(Method)]]로  structuredClone()를 사용하면 한번에 깊은 복사를 할 수 있다.

```js
const mushrooms1 = {
	amanita: ["muscaria", "viros"],
}

const mushrooms2 = structuredClone(mushrooms1);

mushrooms2.amanita.push("pantherina");
mushrooms1.amanita.pop();

console.log(mushrooms2.amanita) // ['muscaria', 'viros', 'pantherina']
console.log(mushrooms1.amanita) // ['muscaria']
```