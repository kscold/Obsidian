- ES2015에서 새로운 자료구조인 [[Map]]과 Set이 추가되었다.

- Set은 [[배열(Array)]]과 유사하다.

- Set은 순서를 보장하지 않는다.

## Set [[메서드(Method)]]
### add()

- 요소(item)를 Set에 추가한다.

## Set 기본 메서드 사용

```js
const s = new Set();
s.add(false); // add로 요소를 Set 객체에 추가
s.add(1);
s.add('1');
s.add(1);
s.add(2);

console.log(s.size); // 중복이 제거되어 4가 프린트

s.has(1); // has로 요소가 존재하는지 여부를 확인
console.log(s.has(1)); // true

for (const a of s) {
	console.log(a); 
}

s.forEach((a) => {
	console.log(a); 
})

// for of 사용
// >> false 1 '1' 2 

// forEach 사용
// >> false 1 '1' 2

s.delete(2); // 2 요소 제거
s.clear() // 모든 요소 제거
```

## Set 객체를 [[배열(Array)]]로 형변환하는 방법

- [[배열(Array)]] [[메서드(Method)]] 중에 Array.from()를 사용하여 배열로 다시 변환할 수 있다.

```js
const arr = [1, 2, 3, 2, 3, 5, 2]
const s2 = new Set(arr) // 중복 제거

console.log(s2)
// >> {1, 2, 3, 5}

Array.from(s2)
```