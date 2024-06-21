- ES2015에서 새로운 자료구조인 Map과 [[Set]]이 추가되었다.

- Map은 [[객체(Object)]]와 유사하다.

- Map은 1 대 1로 매칭되는 관계로 이루어져 있다.
- 즉, 코드를 순서대로 실행되도록 만들 수 있다.
- 따라서 [[객체(Object)]] [[속성(Property)]]간의 순서도 보장된다.

- key와 value를 설정할 때 string와 number 상관없이 사용할 수 있다.

## Map의 [[메서드(Method)]]

### [[set()]]

- [[매개변수(parameter)]]로 key와 value를 지정하여 Map [[객체(Object)]]에 [[속성(Property)]]을 추가한다.
### get()

- key를 사용하여 Map [[객체(Object)]] [[속성(Property)]]을 조회할 때 사용한다.
### size()

- size() [[메서드(Method)]]를 사용하여 [[속성(Property)]] 갯수를 조회한다.

### has()

- [[속성(Property)]] 중에 있는 key인지 존재 여부를 확인힌다.
### delete()

- key를 사용하여 [[속성(Property)]]을 삭제한다.
### clear()

- 요소(item)를 전부 삭제한다.

## Map 기본 메서드 사용

```js
const m = new Map();

m.set('a', 'b'); // set(키, 값)으로 Map에 속성을 추가
m.set(3, 'c'); // 문자열이 아님 숫자 값을 key로 사용 가능

const d = {};
m.set(d, 'e'); // 객체도 사용 가능

m.get(d); // get(key)로 속성값을 조회
console.log(m.get(d)); // e

m.size; // size로 속성 개수를 조회
console.log(m.size); // 3
```


## Map 객체를 반복문 통해 조회

- 밑의 코드는 [[for]]문과 [[forEach()]]를 사용하여 Map [[객체(Object)]]를 순회하여 [[속성(Property)]]을 조회하는 방법이다.

```js
for (const [k, v] of m) { // 반복적인 객체를 순회하기 위해 for ... of로 key, value 추출
	console.log(k, v)
}

// >> 'a', 'b', 3, 'c', {}, 'e'

m.forEach((v, k) => { // forEach도 사용가능
	console.log(k, v); 
})

// >> 'a', 'b', 3, 'c', {}, 'e'
```

## WeakMap

- 메모리에서 [[가비지 콜렉터(garbage collector)]]가 작동이 잘되는 Map [[객체(Object)]]이다.
- 즉, [[가비지 콜렉터(garbage collector)]]가 작동될 때 WeakMap [[객체(Object)]]로 연관된 key와 value가 같이 메모리에서 정리되게 만들 수 있다.

```js
const wm = new WeakMap(); // WeakMap 객체 생성
const obj = {}; // 빈 객체 정의

wm.set(obj, '123'); // key, value 추가

obj3 = null; // null로 만드는 순간 바로 메모리에서 해제, 즉, wm 모든 요소도 해제
```