
arr.find(callback(element, index, array), thisArg)
.[[findIndex()]]와 매개변수는 동일

**find** 함수는 배열의 요소를 순차적으로 순회하면서 조건에 일치하는 **요소의 값**을 즉시 반환 한다.
조건을 일치하는 경우가 없다면, **undefined**를 반환한다.

파라미터 설명

**arr**

- 순회하고자 하는 배열

**element**

- 현재 배열의 요소

**index(생략 가능)**

- 현재 배열 요소의 index