- [[원시 타입(Primitive type)]]을 테스트하는 것이 아니라 [[참조 타입(Reference Type)]]을 테스트할 때 쓰는 [[Jest]] [[메서드(Method)]]이다.
## 예시

- 다음과 같이 아이디를 넘기면 가짜 유저 [[객체(Object)]]를 리턴하는 [[함수(Function)]]를 테스트하려고 합니다.

```js
function getUser(id) {
  return {
    id,
    email: `user${id}@test.com`,
  };
}
```

- 만약 방식으로 다음과 같이 [[toBe()]] 함수를 사용하면 테스트가 실패하는 것을 알 수 있다.

```js
test("return a user object", () => {
  expect(getUser(1)).toBe({
    id: 1,
    email: `user1@test.com`,
  });
});
```

- [[Jest]]는 toBe() 대신에 toEqual() 함수를 사용하라고 가이드해주고 있다. 
- 실제로 테스트 코드를 작성할 때는 [[객체(Object)]]를 검증해야할 일이 많기 때문에 toEqual() 함수를 가장 많이 접할 수 있다.

```bash
$ npm test

> my-jest@1.0.0 test /my-jest
> jest

 FAIL  ./test.js
  ✕ return a user object (7ms)

  ● return a user object

    expect(received).toBe(expected) // Object.is equality

    Expected: {"email": "user1@test.com", "id": 1}
    Received: {"email": "user1@test.com", "id": 1}

    Difference:

    Compared values have no visual difference. Note that you are testing for equality with the stricter `toBe` matcher using `Object.is`. For deep equality only, use `toEqual` instead.

      11 |
      12 | test('return a user object', () => {
    > 13 |   expect(getUser(1)).toBe({
         |                      ^
      14 |     id: 1,
      15 |     email: `user1@test.com`
      16 |   });

      at Object.toBe (test.js:13:22)

Test Suites: 1 failed, 1 total
Tests:       1 failed, 1 total
Snapshots:   0 total
Time:        0.901s, estimated 1s
Ran all test suites.
npm ERR! Test failed.  See above for more details.
```

- 다음과 같이 Matcher 함수 부분을 `toEqual()`로 교체하면 테스트는 통과하게 된다.

```js
test("return a user object", () => {
  expect(getUser(1)).toEqual({
    id: 1,
    email: `user1@test.com`,
  });
});
```

```bash
$ npm test

> my-jest@1.0.0 test /my-jest
> jest

 PASS  ./test.js
  ✓ return a user object (3ms)

Test Suites: 1 passed, 1 total
Tests:       1 passed, 1 total
Snapshots:   0 total
Time:        0.91s, estimated 1s
Ran all test suites.
```