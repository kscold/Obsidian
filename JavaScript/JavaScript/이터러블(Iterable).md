- 이터러블(Iterable)은 `Symbol.iterator` 메서드를 구현하여 `for...of` 루프나 스프레드 연산자(`...`)로 순회할 수 있는 객체다.
- 내장 이터러블: `Array`, `String`, `Map`, `Set`, `arguments`, `NodeList` 등이 있다.
- 이터러블 프로토콜(Iterable Protocol)과 이터레이터 프로토콜(Iterator Protocol)이 분리되어 있다.

## 이터러블 vs 이터레이터

| 구분 | 조건 | 반환값 |
|------|------|--------|
| 이터러블(Iterable) | `Symbol.iterator` 메서드를 가짐 | `next()`를 가진 이터레이터 |
| 이터레이터(Iterator) | `next()` 메서드를 가짐 | `{ value, done }` 객체 |
| 이터러블 이터레이터 | 둘 다 만족 | 자기 자신을 반환 |

```js
const arr = [1, 2, 3];

// 이터러블 → Symbol.iterator 호출 → 이터레이터 반환
const iter = arr[Symbol.iterator]();

console.log(iter.next()); // { value: 1, done: false }
console.log(iter.next()); // { value: 2, done: false }
console.log(iter.next()); // { value: 3, done: false }
console.log(iter.next()); // { value: undefined, done: true }
```

## for...of 동작 원리

- `for...of`는 내부적으로 `[[Symbol]]`.iterator를 호출하여 이터레이터를 얻고, `done`이 `true`가 될 때까지 `next()`를 반복 호출한다.

```js
for (const val of [10, 20, 30]) {
  console.log(val); // 10, 20, 30
}
// 위 코드는 내부적으로 아래와 동일하게 동작한다
const iter = [10, 20, 30][Symbol.iterator]();
let result;
while (!(result = iter.next()).done) {
  console.log(result.value);
}
```

## 커스텀 이터러블 구현

- `[Symbol.iterator]()` 메서드를 직접 구현하여 어떤 객체든 이터러블로 만들 수 있다.

```js
const range = {
  from: 1,
  to: 3,
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

console.log([...range]); // [1, 2, 3]
for (const n of range) console.log(n); // 1, 2, 3
```

## 스프레드와 구조 분해 할당

- 이터러블은 `...` 스프레드 연산자와 구조 분해 할당에도 활용된다.

```js
const [first, ...rest] = new Set([1, 2, 3, 4]);
console.log(first); // 1
console.log(rest);  // [2, 3, 4]
```

## 관련 개념

- [[Symbol]] — `Symbol.iterator`는 이터러블 프로토콜의 핵심 심볼이다.
- [[제너레이터(Generator)]] — `function*`로 선언하며, 자동으로 이터러블 이터레이터를 구현한다.
- [[배열(Array)]] — 가장 대표적인 내장 이터러블이다.
