- 배열은 [ ]로 감싸서 나타내고,  [[객체(Object)]] [[리터럴(literal)]]처럼 안에는 무엇이든지 다 들어갈 수 있다.
- 배열 안에 배열이 들어가도 되고, 배열 안에 객체가 들어가도 된다. 

- 자바스크립트는 [[일급 객체(First Class Object)]]와 [[일급 함수(First Class Function)]]의 특징을 가지기 때문에 들어갈 수 있다.
- 배열 안에 들어간 것들을 우리는 요소(item)이라고 부른다. 
- 객체의 속성처럼 쉼표로 구분하면 된다.

## 자바스크립트 배열의 특징

- 다른 프로그래밍 언어와 다르게 자바스크립트는 배열의 길이를 미리 정할 필요가 없다. 

```jsx
var array = [];
var array2 = [1, "Hello", [1,2,3], { hi: 1 }];
```

- 안의 요소에 따라 자동으로 늘어나거나 줄어든다.
- [[객체(Object)]]와의 가장 큰 차이점은 직접적인 키(key)가 없고그냥 값들만 **순서**대로 나열되어 있는 인덱스(index)가 있다.


- 배열도 배열 [[리터럴(literal)]]인 `[]`를 사용해서 만들 수 있다.

```jsx
var array = new Array();
array[0] = 1;
array[1] = 'Hello'; // 자유롭게 배열을 확장 축소할 수 있음
```


## 배열 [[메서드(Method)]] 정리

### 배열.length

```jsx
var array = [1, 2, 3, 4];
array.length; // 4
```

- 문자열처럼 배열의 길이를 알려준다.
- 배열의 길이는 배열에 요소가 추가되고 제거됨에 따라 자동으로 바뀐다.

### 배열.join(구분자)

```jsx
var array = [1, 2, 3];
array.join(); // "1,2,3"
array.join(':'); // "1:2:3"
```

- 배열의 항목들을 구분자를 기준으로 합친 새 문자열을 반환한다. 
- 구분자를 입력하지 않으면 자동으로 쉼표이다.

### 배열.concat(합칠 것)

```jsx
var array = [1,2,3];
array = array.concat(4,5); // [1,2,3,4,5]
array.concat([6,7]); // [1,2,3,4,5,6,7]
```

- 배열을 합친 새 배열을 반환한다.

### 배열.reverse()

```jsx
var array = [1, 2, 3, 4];
array.reverse(); // [4, 3, 2, 1]
```

- 원래의 배열을 뒤집어버린다,

### **배열.push(항목), 배열.pop()**

```jsx
var array = [1,2,3];
array.push(5); // 4
array; // [1,2,3,5]
array.pop(); // 5
```

- 배열의 뒤에 추가하거나 뺀다.
- 기존의 배열이 변한다. 
- push는 push한 후 변한 배열의 길이를 반환한다.
- pop은 마지막 요소를 제거한 후 그 요소를 반환한다.

### 배열.unshift(항목), 배열.shift()

```jsx
var array = [1,2,3];
array.unshift(0); // 4
array; // [0,1,2,3]
array.shift(); // [0]
```

- 배열의 앞에 추가하거나 뺀다.
- 기존의 배열이 변한다. 
- unshift는 변한 배열의 길이를 반환한다.(앞에서 동작하는 변한 .length 메서드라고 생각하면 된다.)
- shift는 shift된 항목을 반환한다.(앞에서 동작하는 pop이라고 생각하면 된다.)

### 배열.[[splice()]] 파라미터(시작점, 지울 개수, 넣을 것)

```jsx
var array = [1,2,3,4];
array.splice(2, 1, 5); // 3
array; // [1,2,5,4];
```

- pop, shift 같은 것은 배열의 처음 또는 끝만을 뺄 수 있지만, splice는 중간도 뺄 수 있다. 
- 또한 동시에 그 자리에 무언가를 넣을 수도 있다.
- 위 코드 예시를 보면 2번째 자리부터 한 개를 지우고 그 대신에 숫자 5를 넣는 동작이다. 
- 그래서 2번째 자리(3)가 없어지고 5가 들어간다.
- 없애지는 않고 추가만 하고 싶으면 splice(시작점, 0, 넣을 것) 하면 아무것도 없어지진 않고 추가만 된다.

### 배열.[[map()]](function(값, 자릿수) { 조건 }), 배열.forEach(function(값, 자릿수) { 조건 })

```jsx
var array = [1,2,3];
array.map(function(x) {
  return x+1;
}); // [2,3,4]
array.forEach(function(x, i) {
  alert(x + ':' + i);
});
```

배열의 항목들을 반복하면서 조작하는 함수입니다. map과 forEach의 매개변수로 함수가 들어가는데 함수 안에 배열의 항목들을 어떻게 조작할 지 적어주면 됩니다. 예시에서는 각각 1을 더하는 것과 alert하는 조작을 했습니다. map과 forEach의 차이점은 map은 바뀐 새 배열을 반환하지만, forEach는 반환하지 않습니다. 따라서 forEach보다는 map이 활용성이 더 높습니다.

### 배열.reduce(function(이전값, 현재) { 조건 }), 배열.reduceRight

```jsx
var array = [1, 2, 3, 4, 5];
array.reduce(function(prev, cur) {
  return prev + cur;
}); // 15
```

배열을 왼쪽부터 조건을 적용해 하나의 값으로 만듭니다. 위의 코드의 조건은 이전 값과 현재 값을 더한 값을 반환하는 거죠? 왼쪽 두 개부터 시작합니다. 이전 값은 1이고 현재 값은 2라서 더하면 3이 됩니다. 그 값이 다시 이전 값이 됩니다. 이제 이전 값은 3이고 현재 값은 3이라서 더하면 6이되고, 다음에는 이전 값이 6이고, 현재 값은 4가 되어서 더하면 10, 마지막으로 이전 값이 10이고 현재 값이 5라서 더하면 15가 됩니다. 최종 결과를 반환합니다. 간단히 말하면 조건에 따라, 1 + 2 + 3 + 4 + 5를 한 겁니다. 오른쪽부터 줄여가고 싶으면 자매품, reduceRight 함수를 사용하시면 됩니다.

### **배열.filter(function(항목) { 조건 })**

```jsx
var array = [1,2,3,4,5];
array.filter(function(x) {
  return x % 2 === 0;
}); // [2,4]
```

필터링한다는 말을 들어보셨죠? 특정 조건에 해당하는 배열만을 걸러내 새 배열로 만듭니다. 역시 매개변수로 있는 함수 안에 조건을 적으면 됩니다. 여기서는 조건이 항목을 2로 나눈 나머지가 0인 것(짝수)만 걸러내라고 되어있네요.

### 배열.sort(function(이전 값, 다음 값) { 조건 })

```jsx
var array = [5,2,3,4,1];
array.sort(function(x,y) {
  return x - y;
}); // [1,2,3,4,5]
```

배열을 특정 조건에 따라 정렬합니다. 조금 복잡합니다.

### 배열.indexOf(찾을 것), 배열.lastIndexOf(찾을 것)

문자열처럼 배열에서 찾습니다. 여러 개가 있더라도 처음으로 찾은 위치만을 알려줍니다. lastIndexOf는 뒤에서부터 찾습니다.

### 배열.every(function(항목) { 조건 }), 배열.some(function(항목) { 조건 })

```jsx
var array = [1, 3, 5, 7, 9];
array.every(function(i) {
  return i % 2 === 1;
}); // true
array.every(function(i) {
  return i < 9;
}); // false
array.some(function(i) {
  return i === 9;
}); // true
```

최근에 추가된 함수인데 생각보다 유용해서 소개합니다. 각각 배열의 모든 항목 또는 일부 항목이 true면 true를 반환합니다. 즉 **every**는 모든 항목이 조건을 만족하면 true, **some**은 하나의 항목이라도 조건을 만족하면 true를 반환입니다. every의 첫 예는 array의 모든 값이 다 홀수이기 때문에 true가 됩니다. 하지만 그 다음 예는 `i < 9`를 다 만족하지는 않기 때문에 false가 되죠. some의 예시에서는 배열 요소 중 9인 게 하나 이상 있기 때문에 true를 반환합니다.

### Array.isArray(값)

```jsx
Array.isArray('array?'); // false
Array.isArray(['array?']); // true
```

Array 객체 자체의 static 메소드입니다. 배열인지 아닌지 확인해주는 역할을 합니다. 생각보다 자주 쓰이니 기억해둡시다.