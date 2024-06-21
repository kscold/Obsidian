- [[함수(Function)]]를 만드는 [[키워드(Keyword)]]이다.

- [[호이스팅(variable hoisting)]]이 이루어지는 함수 선언식에 많이 사용된다.
- 특히 [[생성자 함수(Constructor Function)]]는 무조건 function [[키워드(Keyword)]]로 선언해야 한다.


## function 키워드의 속성

- function [[키워드(Keyword)]]를 사용하면 [[생성자(Constructor)]]에 따라서 아래와 같은 [[속성(Property)]]들이 만들어진다.
### arguments 

- 함수가 호출될 때 전달된 인자를 담은 [[배열(Array)]]이다.
### caller

- 함수를 호출한 [[함수(Function)]]이다.
### length

- 함수의 [[매개변수(parameter)]]의 개수를 의미한다.
### [[prototype]]

- 함수 [[객체(Object)]]에만 있고, [[생성자 함수(Constructor Function)]]로 [[객체(Object)]]를 생성할 때 사용한다.


## function 키워드의 [[this]]

- [[function]] [[키워드(Keyword)]]를 사용하면 [[this]]의 범위는 전역이다.
- 따라서 [[메서드(Method)]]의 두 번째 [[매개변수(parameter)]]를 이용하여 [[this]]를 지정해 줄 수 있다.
- 다른 방법으로는 [[화살표 함수(Arrow function)]]를 사용하면 된다.

```js
const audio = {
	title: 'audio',
	categories: ['rock', 'pop', 'hiphop'],
	
	displayCategories() {
		this.categories.forEach(
			function (category) {
				console.log(`this.title ${this.title}, category: ${category}`);
			},
			{ title: 'audio' } // 또는 그냥 this라고 지정해도 됨
		);
	},
};

audio.displayCategories();
```

- 위의 코드처럼참조 범위가 전역이므로 this 범위가 달라 [[undefined]]가 뜨기 때문에 두 번째 [[매개변수(parameter)]]를 주어 [[this]] 값을 정확히 지정할 수 있다.