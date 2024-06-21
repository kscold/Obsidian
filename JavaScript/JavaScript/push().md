- push( )함수는 기존 [[배열(Array)]]에 원소(item)를 추가하며 [[배열(Array)]]의 총 길이를 반환한다.


## 예시

```jsx
// Array.push(addItem1, addItem2...) 
// 신규 원소를 넣은 후 총 길이를 리턴한다 
const aa = ["lion", "tiger"]; 
const bb = aa; 
bb.push("cat", "dog");

// aa.push(["cat", "dog"]); 
// 이렇게 하면 Item이 아닌 Array가 들어가므로 주의!

console.log(aa); // ["lion", "tiger", "cat", "dog"] 
console.log(bb); // ["lion", "tiger", "cat", "dog"] 
console.log(Object.is(aa, bb)); // true
```

- 따라서 위의 코드 예시의 Object.is(aa, bb)를 하면 같은 [[참조 타입(Reference Type)]]이라는 것을 알 수 있다.