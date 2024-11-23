- concat( ) 함수는 기존 [[배열(Array)]]을 복사한 후 원소를 추가하며 새 배열을 리턴한다.
- 즉, 새로운 [[객체(Object)]]를 만든다.

## 예시

```jsx
// Array.concat(addItem1, addItem2...) 
const aa = ["lion", "tiger"];
const bb = aa.concat("cat", "dog"); // Combines two or more arrays. 

// aa.push(["cat", "dog"]); 
// 이렇게 하면 Item이 아닌 Array가 들어가므로 주의! 

console.log(aa); // ["lion", "tiger"] 
console.log(bb); // ["lion", "tiger", "cat", "dog"] 
console.log(Object.is(aa, bb)); // false
```

- 위의 예시 코드를 보면 다른 [[참조 타입(Reference Type)]]을 가지는 것을 볼 수 있다.