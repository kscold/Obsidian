- 자바스크립트의 for문은 다양하게 사용가능하다.


## 기본 for문 사용

- 가장 일반적인 for문의 형태는 다음과 같다.  

- 밑의 코드는 console.log(i)를 총 10번 반복하는 반복문이다.

```js
for(let i = 0; i<10; i++){
	console.log(i);
}
```

- 위의 for문 안의 구성이 각각 의미하는 바를 살펴보면 다음과 같다.
	- 변수 let i를 선언하고 0으로 할당한다. (반복문 초기값 설정한다.) 
	- for문을 얼마나 돌릴 것인지 조건을 명시한다.( 조건이 참이면 실행한다.)  
	- 루프가 한번 돌때마다 변수 증감식을 실행한다.

- 여기에서는 i가 0부터 시작해서 10보다 작을 때까지 이므로 0~9. 총 10번 반복할 것이다.  
- i의 값은 루프가 한 번 돌때마다 1씩 증가하여 console.log는 1, 2, 3, .... 9를 순서대로 출력하고 for문은 종료 된다.

- 그러나 위의 기본적인 for문 문법을 브라우저에서 사용할 경우 [[prototype]]와 같은 [[객체(Object)]] [[속성(Property)]]까지 전부 순회하는 문제가 발생할 수 있다.
- 따라서 밑에 나오는 for ... in 문법에 .hasOwnProperty() [[메서드(Method)]]를 사용해주면 해결된다.


## for ... in

- for in문은 [[객체(Object)]]에 사용할 수 있는 반복분이다.  
- [[객체(Object)]] [[속성(Property)]]을 따라서 반복한다.
- [[배열(Array)]]에도 사용할 수 있지만 배열 반복에는 추천되지 않는다.

```js
const obj = {
	name : '이름',
	age : '나이'
}

for(const key in obj){ // 기본적으로 key를 순회
	console.log(key); // key값 출력
	console.log(obj.name, obj.age); // value 값 출력
	
	console.log(`key 값 : ${key}`); 
	console.log(`value 값 : ${obj[key]}`); 
}

// >> name
// >> age
// >> '이름', '나이'
// >> key값 : name
// >> key값 : age
// >> value 값 : 이름
// >> value 값 : 나이
```

- 기본 for문과 생김새는 비슷하다.
- 대신 in [[키워드(Keyword)]]를 사용한다.  

- obj.name과 같이 사용하려면 [[객체(Object)]]내에 name이라는 key값을 가진 value가 존재해야 한다.
- 없는 key값을 사용하게 되면 [[undefined]]가 출력된다.  

- [[객체(Object)]]의 모든 value값을 얻으려면 obj`[key]` 문법을 사용하여 모든 [[속성(Property)]]을 순회하여 얻을 수 있다.

- 브라우저에서 [[prototype]]과 같은 [[속성(Property)]]을 무시하고 [[속성(Property)]]을 순회하는 방법은 다음과 같다.
- for ... in 구문 안에 .hasOwnProperty() [[메서드(Method)]]를 사용해 조건을 걸어주면 된다.

```js
const obj2 {
	... // 여러 속성들 정의
}

for (let i in obj2) {
	if (obj2.hasOwnProperty(i)) { // 가지고 있는 속성만 표현
		obj2[i];
	}
}
```


## for...of

- for of문은 반복 가능한 [[객체(Object)]]([[배열(Array)]], [[Map]], [[Set]], String, TypedArray, arguments 객체 등)에 대해 사용할 수 있다. 
- 보통은 [[배열(Array)]]에 사용한다고 흔히 알려져 있다.

```js
const array = ['1번', '2번', '3번'];

for(const element of array) {
	console.log(element); // 배열[0] ~ 끝까지 순차적 출력
	console.log(array); // 배열 전체 출력
}
```

- [[배열(Array)]]에 들어있는 0번째 ~ 마지막 번째 요소까지 순차적으로 출력된다.  
- [[배열(Array)]] 안에 있는 요소를 꺼내쓸 때 사용하면 좋다.