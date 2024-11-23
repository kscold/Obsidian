- [[Shift()]] [[메서드(Method)]]는 [[배열(Array)]]에서 첫 번째 요소를 제거하고, 제거된 요소를 반환한다.
- 파이썬의 pop()과 같다고 생각하면 된다.

- 이 메서드는 [[배열(Array)]]의 길이([[length]])를 변하게 한다.


## 문법

```js
arr.shift();
```


- [[배열(Array)]]에서 제거한 요소. 빈 [[배열(Array)]]의 경우 [[undefined]] 를 반환한다.

- shift() [[메서드(Method)]]는 0번째 위치의 요소를 제거 하고 연이은 나머지 값들의 위치를 한칸 씩 앞으로 당긴다.

- 그리고 제거된 값을 반환 한다.
- 만약 [[배열(Array)]]의 [[length]]가 0이라면 [[undefined]]를 반환한다.


## 배열에서 한 요소 제거하기

- 아래 코드는 `myFish` 라는 [[배열(Array)]]에서 첫번째 요소를 제거 하기 전과 후를 보여 준다.
- 그리고 제거된 요소도 보여준다.

```js
var myFish = ["angel", "clown", "mandarin", "surgeon"];

console.log("myFish before: " + myFish);
// "제거전 myFish 배열: angel, clown, mandarin, surgeon"

var shifted = myFish.shift();

console.log("myFish after: " + myFish);
// "제거후 myFish 배열: clown, mandarin, surgeon"

console.log("Removed this element: " + shifted);
// "제거된 배열 요소: angel"
```

## while 반복문 안에서 shift() 사용하기

- shift() 메서드는 while 문의 조건으로 사용되기도 한다. 
- 아래 코드에서는 while 문을 한번 돌 때 마다 배열의 다음 요소를 제거하고, 이는 빈 배열이 될 때까지 반복된다.

```js
var names = ["Andrew", "Edward", "Paul", "Chris", "John"];

while ((i = names.shift()) !== undefined) {
  console.log(i);
}

// Andrew, Edward, Paul, Chris, John
```