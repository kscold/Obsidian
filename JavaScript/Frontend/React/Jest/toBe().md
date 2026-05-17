- `toBe()`는 [[Jest]]에서 값이 기대값과 일치하는지 `Object.is` 알고리즘으로 검사하는 매처(matcher)이다.
- 원시값(숫자, 문자열, 불리언, null, undefined) 비교에 적합하며, 객체는 참조(reference) 동일성을 검사한다.

## 기본 사용법

```ts
test('숫자 덧셈', () => {
  expect(1 + 2).toBe(3);         // 원시값 비교 → 통과
  expect('hello').toBe('hello'); // 문자열 비교 → 통과
  expect(true).toBe(true);       // 불리언 비교 → 통과
});
```

## Object.is 기반 비교

- `toBe()`는 내부적으로 `Object.is(received, expected)`를 사용한다.
- `===`와 거의 같지만 `NaN`과 `-0` 처리에서 차이가 있다.

```ts
test('Object.is 특이 케이스', () => {
  expect(NaN).toBe(NaN);   // 통과 (Object.is는 NaN === NaN 을 true로 처리)
  expect(-0).toBe(+0);     // 실패 (Object.is는 -0 !== +0)
});
```

## toBe() vs [[toEqual()]]

| 구분 | toBe() | toEqual() |
|------|--------|-----------|
| 비교 방식 | 참조(reference) 동일성 | 값(value) 내용 재귀 비교 |
| 원시값 | 적합 | 가능하지만 toBe() 권장 |
| 객체/배열 | 같은 레퍼런스여야 통과 | 내용이 같으면 통과 |

```ts
test('객체 비교 차이', () => {
  const obj = { a: 1 };

  expect(obj).toBe(obj);          // 통과 (같은 참조)
  expect(obj).toBe({ a: 1 });     // 실패 (다른 참조)
  expect(obj).toEqual({ a: 1 }); // 통과 (내용 동일)
});
```

## [[expect()]] 와 함께 사용

```ts
test('함수 반환값 검사', () => {
  const add = (a: number, b: number) => a + b;

  expect(add(2, 3)).toBe(5);
  expect(add(0, 0)).toBe(0);
  expect(add(-1, 1)).toBe(0);
});
```

## not.toBe() — 불일치 검사

```ts
test('다른 값인지 확인', () => {
  expect(1 + 1).not.toBe(3); // 1+1은 3이 아님 → 통과
});
```

## 활용 팁

- 함수 반환값이 특정 원시값인지 확인할 때 `toBe()` 사용
- 객체나 배열의 내용을 비교할 때는 반드시 [[toEqual()]] 사용
- 참조 동일성이 필요한 싱글톤, 캐시 테스트에도 `toBe()` 활용 가능
