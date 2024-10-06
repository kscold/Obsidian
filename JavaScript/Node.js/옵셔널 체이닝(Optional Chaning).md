- 옵셔널 체이닝 연산자는 null이나 [[undefined]]의 [[속성(Property)]]을 조회하는 경우에 에러가 발생하는 것을 막는다.
- 따라서 에러를 안나는 대신 [[undefined]]가 할당된다.

## 옵셔널 체이닝을 사용하는 경우

- 아래 코드는 옵셔널 체이닝을 사용하지 않은 코드이다.

```js
const a = []
a.b // a가 객체이므로 오류가 나지 않음

const c = null;
try{
	c.d;
} catch (e) {
	console.error(e); // TypeError: Cannot read propertie of null (reading 'd')
}
```

- 또한 위의 코드에서 주의해야 할 점은 .d의 [[속성(Property)]]이 정의되지 않아서 TypeError: Cannot read propertie of null (reading 'd') 오류가 난 것이 아니라 정확히는 [[객체(Object)]] c가 null이기 때문에 오류가 난 것이다.

- 아래 코드는 옵셔널 체이닝을 사용한 경우이다.
- 옵셔널 체이닝은 [[배열(Array)]]에도 사용할 수 있다.

```js
c?.d // 에러가 안나는 대신 undefined가 할당됨

try{
	c.f();
} catch (e) {
	console.error(e); // TypeError: Cannot read propertie of null (reading 'f')
}

c?.f(); // 에러가 안나는 대신 undefined가 할당됨

try{
	c[0];
} catch (e) {
	console.error(e); // TypeError: Cannot read propertie of null (reading '0')
}

c?.[0]; // 에러가 안나는 대신 undefined가 할당됨
```