- fill()은 [[배열(Array)]]의 [[메서드(Method)]]이다.

- 원하는 값을 [[배열(Array)]]의 원하는 요소 자리에 채우고 싶을 때  fill() 함수를 사용한다.
- 파이썬의 range()와 비슷하다.


## fill() 기본 형식


```js
arr.fill(value, start, end);
```

- 지정한 value 값을 채운 배열이 반환된다.
- value값을 시작 인덱스부터 끝 인덱스 '전'까지 채우는 형식이다. 
- 끝 인덱스까지 채워지는 게 아니니 주의하여야 한다.
### value

- 해당 배열 위치에 넣을 필수 값이다.
### start(옵션)

- 시작 인덱스이며 기본값=0 이다.
### end(옵션)

- 끝 인덱스이며 기본값=배열의 [[length]]이다.


## Array()와  Array.fill()의 차이

- Array().fill()과 Array(5)의 차이는 [[배열(Array)]]의 초기 상태와 요소의 값을 어떻게 채우는지에 있다.

```js
const arr = Array(5);

console.log(arr); // [ <5 empty items> ]
```

- Array(5)는 길이가 5인 빈 배열을 생성한다.
- 이 배열은 요소가 초기화되지 않은 상태로, "sparse" 배열이라고도 한다.
- console.log()로 출력하면, 각 요소가 [[undefined]]로 초기화된 것이 아니라 아예 존재하지 않는 것으로 나타난다.

- 이 배열에 [[map()]], [[forEach()]], [[reduce()]] 등과 같은 배열 메서드를 바로 사용하면, 이러한 메서드들은 존재하지 않는 요소들을 건너뛰게 됩니다.

```js
const arr = Array(5).fill(); 

console.log(arr); // [ undefined, undefined, undefined, undefined, undefined ]
```

- `Array(5).fill()`은 길이가 5인 배열을 생성하고, 각 요소를 [[undefined]]로 채운다.

- 이 배열의 각 요소는 존재하며, [[undefined]] 값을 갖는다.
- 이 배열에 [[map()]], [[forEach()]], [[reduce()]] 등을 사용하면, 모든 요소가 [[undefined]] 값을 가지기 때문에 이러한 메서드들이 모든 요소를 처리한다.


## 예시

```js
const arr1 = ['a', 'b', 'c', 'd'];

console.log(arr1.fill('z', 1, 3));

// ["a", "z", "z", "d" ]

const arr2 = ['a', 'b', 'c', 'd'];

console.log(arr2.fill('z', 1));

// ["a", "z", "z", "z" ]

const arr3 = ['a', 'b', 'c', 'd'];

console.log(arr3.fill('z'));

// ["z", "z", "z", "z" ]
```

- 다음은 음수 인덱스 값 예시이다.

```js
const arr4 = ['a', 'b', 'c', 'd'];

console.log(arr4.fill('z', -3, -1));

// ["a", "z", "z", "d" ]

const arr5 = ['a', 'b', 'c', 'd'];

console.log(arr5.fill('z', -3));

// ["a", "z", "z", "z" ]
```

- 다음은 [[배열(Array)]] 생성 후 fill()로 값을 채우는 예시이다.

```js
const arr6 = Array(5).fill();

console.log(arr6);

// [undefined, undefined, undefined, undefined, undefined]

const arr7 = Array(5).fill('a');

console.log(arr7);

// ['a', 'a', 'a', 'a', 'a']

const arr8 = Array(5).fill().map((item, index) => index + 1); 

console.log(arr8);

// [1, 2, 3, 4, 5]
```