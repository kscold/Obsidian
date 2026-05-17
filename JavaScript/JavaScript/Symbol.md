- Symbol은 ES2015에서 도입된 [[원시 타입(Primitive Type)]]으로, 생성할 때마다 유일한(unique) 값을 반환한다.
- `Symbol()` 함수로 생성하며, `new` 키워드를 사용하지 않는다.
- 같은 설명 문자열을 넣어도 두 Symbol은 절대 같지 않다(`Symbol('id') !== Symbol('id')`).
- 주로 [[객체(Object)]]의 숨겨진 [[속성(Property)]] 키로 사용하거나 충돌 없는 상수 정의에 활용한다.

## Symbol 생성

- `Symbol(description)`: 설명을 붙인 고유한 Symbol 생성 (설명은 디버깅용, 동등 비교에 영향 없음)
- `Symbol.for(key)`: 전역 레지스트리에서 동일 key의 Symbol을 공유 (같은 key면 동일 Symbol)
- `Symbol.keyFor(sym)`: 전역 레지스트리에 등록된 Symbol의 key 반환

```js
const id1 = Symbol('id');
const id2 = Symbol('id');
console.log(id1 === id2); // false — 항상 고유

const shared1 = Symbol.for('token');
const shared2 = Symbol.for('token');
console.log(shared1 === shared2); // true — 전역 레지스트리 공유

console.log(Symbol.keyFor(shared1)); // 'token'
```

## 객체 프로퍼티 키로 사용

- Symbol을 키로 사용하면 `for...in`, `Object.keys()`, `JSON.stringify()`에 노출되지 않아 "숨겨진 프로퍼티" 효과를 낸다.
- 다른 라이브러리 코드와 키 충돌 없이 메타데이터를 저장할 때 유용하다.

```js
const SECRET_KEY = Symbol('secretKey');

const user = {
  name: '홍길동',
  [SECRET_KEY]: 'supersecret123',
};

console.log(user[SECRET_KEY]); // 'supersecret123'
console.log(Object.keys(user)); // ['name'] — Symbol 키는 제외
```

## 잘 알려진 Symbol (Well-known Symbols)

- `Symbol.iterator`: [[객체(Object)]]를 [[이터러블(Iterable)]]로 만드는 이터레이터 메서드 정의. [[for...of]]와 스프레드에서 사용
- `Symbol.toPrimitive`: [[객체(Object)]]를 [[원시 타입(Primitive Type)]]으로 변환할 때 호출
- `Symbol.hasInstance`: `instanceof` 연산자 동작 커스터마이징

```js
const range = {
  from: 1,
  to: 5,
  [Symbol.iterator]() {
    let current = this.from;
    const last = this.to;
    return {
      next() {
        return current <= last
          ? { value: current++, done: false }
          : { value: undefined, done: true };
      },
    };
  },
};

for (const num of range) {
  console.log(num); // 1, 2, 3, 4, 5
}
```
