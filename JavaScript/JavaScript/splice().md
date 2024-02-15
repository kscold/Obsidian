- splice() [[메서드(Method)]]드는 [[배열(Array)]] [[객체(Object)]]에 사용할 수 있는 내장 [[메서드(Method)]]로 이는 기존 요소(iterm)를 삭제하거나 교체하여 [[배열(Array)]]의 내용을 변경하며, 제거된 요소가 담긴 별도의 배열을 새로 반환한다.
## 문법

```js
array.splice(start[, deleteCount[, item1[, item2[, ...]]]])
```
### start

- 배열의 변경을 시작할 index이다.
- 시작 위치에서 자른다(제거)고 생각하면 편하다.
- 배열의 길이보다 큰 값일 경우 배열의 길이(끝)로 설정한다.
- 음수인 경우 배열의 끝에서부터 요소를 센다.
	- 예 ) array.splice(-n) -> array.length -n과 같다.
	- 값의 절대값이 배열의 길이보다 큰 경우 0으로 설정한다.
### deleteCount(Optional)

 - 배열에서 제거할 요소의 수이다.
- 생략하거나 값이 array.length - start보다 클 경우 start 부터의 모든 요소를 제거한다.
- 0이하의 값을  설정 할 경우 어떤 요소도 제거하지 않음, 이 경우 최소한 하나의 새로운 요소를 추가해야한다.
- 제거를 실행하고 변수에 배열을 대입할 시에 제거된 요소가 반환된다.(pop과 같음)
### item1, item2, ...(Optional)

- 배열에 추가할 요소이다.
- 생략할 경우 요소를 제거하기만 한다.

## 예시

- 요소를 제거하지 않고 2번 index에 '딸기', '사과' 추가하는 예시이다.

```js
const fruits = ['수박', '바나나', '망고', '두리안'];

const removed = fruits.splice(2, 0, '딸기', '사과'); // 시작 인덱스2(망고), 0(삭제 안함), ['딸기', '사과']를 추가하여 새로운 배열 fruits으로 반환

console.log(fruits);
// >> ['수박', '바나나', '딸기', '사과', '망고', '두리안'];

console.log(removed); // 두 번째 파라미터 0 이므로 아무것도 반환하지 않는다.
// >> [];
```

- index에서 1개 요소 제거하는 예시이다.

```js
const fruits = ['수박', '바나나', '망고', '두리안'];

const removed = fruits.splice(2, 1); // '망고'를 제거하고 이를 반환함(pop)

console.log(fruits);
// ['수박', '바나나', '두리안'];

console.log(removed); // 제거된 요소를 출력
// ['망고'];
```

- index에서 2개 요소 제거 후 '멜론' 추가하는 예시이다.

```js
const fruits = ['수박', '바나나', '망고', '두리안'];

const removed = fruits.splice(1, 2, '멜론'); 
// 1번째부터 2개의 요소 '바나나', '망고'를 제거 하고 '멜론'을 추가함

console.log(fruits);
// ['수박', '멜론', '두리안'];

console.log(removed); // 삭제된 요소들을 출력
// ['바나나', '망고'];
```

- 끝에서 2번째 요소부터 2개의 요소를 제거하는 예시이다.

```js
const fruits = ['수박', '바나나', '망고', '두리안'];

const removed = fruits.splice(-2, 2); // -2(음수)이므로 뒤에서 인덱스 -1, -2인 '망고' 부터 '망고', '두리안'을 제거함

console.log(fruits);
// ['수박', '바나나'];

console.log(removed);
// ['망고', '두리안'];
```

- index 포함 이후의 모든 요소 제거하는 예시이다.

```js
const fruits = ['수박', '바나나', '망고', '두리안'];

const removed = fruits.splice(1); // '바나나'부터 자름(제거)

console.log(fruits);
// ['수박'];

console.log(removed); // 제거된 요소들을 반환
// ['바나나', '망고', '두리안'];
```