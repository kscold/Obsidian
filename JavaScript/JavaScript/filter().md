- filter 함수를 사용해서 특정 [[배열(Array)]]에서 특정 원소(혹은 요소)만 제외시킬 수 있다.

- Array [[인스턴스(Instance)]]의 filter() [[메서드(Method)]]는 주어진 배열의 일부에 대한 얕은 복사본을 생성하고, 주어진 [[배열(Array)]]에서 제공된 [[함수(Function)]]에 의해 구현된 테스트를 통과한 요소로만 필터링 한다.

- 이처럼 어떤 데이터 집합에서 특정 조건을 만족하는 값만 반환받기를 원할 때 filter() [[메서드(Method)]]를 사용한다.

## 예시

- 자바스크립트에서 filter 는 [[배열(Array)]]에 사용하며, 주어진 함수를 만족하는 모든 요소를 모아 새 [[배열(Array)]]로 반환한다.

- 밑에 코드는 numbers 배열에서 3만 없애는 예시이다.

```jsx
const numbers = [1, 2, 3, 4, 5, 6];
const withoutThree = numbers.filter(number => number !== 3); 
// numbers가 3인 요소를 제외하고 새로운 배열을 만듬

// >> [1, 2, 4, 5, 6]
```