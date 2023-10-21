
splice() 메소드는 배열 객체에 사용할 수 있는 내장 메소드로 이는 기존 요소를 삭제하거나 교체하여 배열의 내용을 변경하며, 제거된 요소가 담긴 별도의 배열을 새로 반환하낟.


```
array.splice(start[, deleteCount[, item1[, item2[, ...]]]])
```

파라미터

1. start
    - 배열의 변경을 시작할 index
    - 배열의 길이보다 큰 값일 경우 배열의 길이로 설정
    - 음수인 경우 배열의 끝에서부터 요소를 센다.
        - 예 ) array.splice(-n) -> array.length -n과 같다.
        - 값의 절대값이 배열의 길이보다 큰 경우 0으로 설정
2. deleteCount(Optional)
    - 배열에서 제거할 요소의 수
    - 생략하거나 값이 array.length - start보다 클 경우 start 부터의 모든 요소를 제거
    - 0이하의 값을  설정 할 경우 어떤 요소도 제거하지 않음, 이 경우 최소한 하나의 새로운 요소를 추가해야한다.
3. item1, item2, ...(Optional)
    - 배열에 추가할 요소
    - 생략할 경우 요소를 제거하기만 한다.

## 예제

### 1. 요소를 제거하지 않고 2번 index에 '딸기', '사과' 추가

```
const fruits = ['수박', '바나나', '망고', '두리안'];

const removed = fruits.splice(2, 0, '딸기', '사과');

console.log(fruits);
// ['수박', '바나나', '딸기', '사과', '망고', '두리안'];

console.log(removed);
// [];
```

### 2. 2번 index에서 1개 요소 제거

```
const fruits = ['수박', '바나나', '망고', '두리안'];

const removed = fruits.splice(2, 1);

console.log(fruits);
// ['수박', '바나나', '두리안'];

console.log(removed);
// ['망고'];
```

### 3. 1번 index에서 2개 요소 제거 후 '멜론' 추가

```
const fruits = ['수박', '바나나', '망고', '두리안'];

const removed = fruits.splice(1, 2, '멜론');

console.log(fruits);
// ['수박', '멜론', '두리안'];

console.log(removed);
// ['바나나', '망고'];
```

### 4. 끝에서 2번째 요소부터 2개의 요소를 제거

```
const fruits = ['수박', '바나나', '망고', '두리안'];

const removed = fruits.splice(-2, 2);

console.log(fruits);
// ['수박', '바나나'];

console.log(removed);
// ['망고', '두리안'];
```

### 5. 1번 index 포함 이후의 모든 요소 제거

```
const fruits = ['수박', '바나나', '망고', '두리안'];

const removed = fruits.splice(1);

console.log(fruits);
// ['수박'];

console.log(removed);
// ['바나나', '망고', '두리안'];
```