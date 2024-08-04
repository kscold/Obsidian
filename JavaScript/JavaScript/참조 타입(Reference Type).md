- 참조 타입은 메모리에 직접 접근이 아닌 메모리의 위치(주소)에 대한 간접적인 참조를 통해 메모리에 접근하는 데이터 타입이다.
- 참조값이라고도 한다.

- [[원시 타입(Primitive type)]]을 제외한 나머지는 참조 타입이다.
- [[함수(Function)]], [[배열(Array)]], [[클래스(Class)]], [[객체(Object)]]가 있다.
- [[원시 타입(Primitive type)]]과 가장 큰 차이점은 [[변수(Variable)]]의 크기가 동적으로 변한다는 것이다. 

- 참조값을 복사할 때는 [[변수(Variable)]]가 [[객체(Object)]]의 참조(Reference)를 가리키고 있기 때문에 복사된 [[변수(Variable)]] 또한 [[객체(Object)]]가 저장된 메모리 공간의 참조를 가리키고 있다.
- 따라서 [[힙(Heap)]]에 저장이 된다.

- 또한 복사를 하고 [[객체(Object)]]를 수정하면 두 [[변수(Variable)]]는 똑같은 참조를 가리키고 있기 때문에 기존 [[객체(Object)]]를 저장한 [[변수(Variable)]]에 영향을 끼친다.
- 이처럼 객체의 참조값(주소값)을 복사하는 것을 [[얕은 복사]]라고 한다.

- [[불변성 유지]]가 되지 않기 때문에 [[확산 연산자(spread operator)]]나 [[배열(Array)]]의 내장 [[메서드(Method)]]들을 적극 활용하는 것이 좋다.


## 참조 타입의 예시

```js
const a = {
  one: 1,
  two: 2,
};
let b = a;

b.one = 3;

console.log(a); // { one: 3, two: 2 } 출력
console.log(b); // { one: 3, two: 2 } 출력
// 기존 값에 영향을 끼친다.
```

- 이러한 특징 때문에 Object의 데이터 자체는 별도의 메모리 공간([[힙(Heap)]])에 저장된다.
- 변수에 할당 시 데이터에 대한 주소([[힙(Heap)]] 메모리의 주소값)가 저장되기 때문에 자바스크립트 엔진이 [[변수(Variable)]]가 가지고 있는 메모리 주소를 이용해서 [[변수(Variable)]]의 값에 접근하게 되는 것이다.

![](https://velog.velcdn.com/images%2Fnomadhash%2Fpost%2F6576b5c3-a064-4f24-a96f-b287b46c2aab%2F%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202020-09-16%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%207.45.04.png)

- 만일 [[let]] myArray = `[]`라는 [[배열(Array)]]을 생성하면 위와 같은 일이 일어난다.
- 위의 예시 그림에서 볼 수 있듯이 [[원시 타입(Primitive type)]]의 값들은 값들이 직접적으로 저장되어 있지만, myArray (참조타입)는 [[힙(Heap)]] 메모리의 주소값이 저장되어 있다.

```jsx
let myArr = [];
let copyArr = myArr;

myArr.push("hello");

console.log(copyArr); // ["hello"]
```

![](https://velog.velcdn.com/images%2Fnomadhash%2Fpost%2Fac894f26-b94a-41f8-990e-8b44c6775d97%2F%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202020-09-16%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%207.54.37.png)

- 위 예시를 보면 알 수 있듯이, 참조 타입의 변수는 실제 데이터가 저장된 주소를 참조하기에 참조(reference) 타입이라고 불리는 것이다.

- 그렇기에 변수의 복사나 수정 시 참조 여부를 잘 고려해야 한다.
- 따라서 [[객체(Object)]]를 복사할 때  =  키워드나 push 같은 [[메서드(Method)]] 사용해서 복사하면 얕은 복사가 돼서 기존 변수 또한 수정되기 떄문에 [[확산 연산자(spread operator)]], [[map()]], [[filter()]], [[slice()]], reduce 등등 새로운 [[배열(Array)]]을 반환하는 [[메서드(Method)]]들을 활용해야 한다.

- 만일 이러한 특성을 고려하지 않은 채 중요한 정보를 담고있는 [[객체(Object)]]나 [[배열(Array)]]에 수정 및 복사를 가하게되면 원본 데이터가 예상치 못한 방향 으로 변경될 수 있으므로 항상 이를 고려하자.


## 참조에 의한 함수 호출 방식

```js
const a  = 100;
const objA = { value: 100 };

function changeArg(num,obj) {
	num = 200;
	obj.value = 200;
	
	console.log(num);
	console.log(obj);
}

changeArg(a,objA);

console.log(a);
console.log(objA);

// 출력값
// 200
// { value : 200 }

// 100
// { value : 200 }
```

- [[원시 타입(Primitive type)]]과 참조 타입의 경우는 함수 호출 방식도 다르다.
- [[원시 타입(Primitive type)]]은 값에 의한 호출(Call By Value)방식으로 동작한다. 

- [[함수(Function)]]의 인자로 [[원시 타입(Primitive type)]]이 넘겨질 경우, 함수의 [[매개변수(parameter)]]로 복사된 값이 전달된다.
- 따라서, [[함수(Function)]] 내부에서 [[매개변수(parameter)]]를 이용해 값을 변경해도, 실제로 호출된 변수의 값이 위 예제처럼 100에서 변경되지 않는다.

- 반면 [[객체(Object)]]와 같은 참조 타입의 경우, 함수를 호출할 때 참조에 의한 호출(Call By Reference) 방식으로 동작한다.
- 즉, 함수를 호출할 때, 객체의 참조값이 전달되고, 함수 내부에서 참조값을 이용해서 인자로 넘긴 실제 객체의 값을 200으로 변경할 수 있다.


## 참조 타입을 [[원시 타입(Primitive type)]]처럼 값으로 비교하는 방법

- [[얕은 복사]]와 달리 [[깊은 복사(Deep Copy)]]는 [[객체(Object)]]의 경우에도 값으로 비교할 수 있다.
- [[객체(Object)]] 깊이(depth)가 깊은 경우는 lodash 라이브러리의 isEqual() [[메서드(Method)]]를 사용하면 된다.

```js
const obj1 = { a: 1, b:2 };
const obj2 = { a: 1, b:2 };

console.log(JSON.stringify(obj1) === JSON.stringify(obj2)); // true
```