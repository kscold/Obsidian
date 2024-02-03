- afterAll()은 [[it()]]으로 동일 레벨 또는 하위 레벨의 [[테스트]]가 실행 후 딱 한번 실행되는 [[Jest]] [[메서드(Method)]]이다.
- 반대로 [[beforeAll()]]도 있다.
## 예시

```jsx
beforeAll(() => {
  return initializeCityDatabase();
});

afterAll(() => {
  clearCityDatabase();
});

describe('One-Time Setup - Vienna, Seoul', () => {
  test('city database has Vienna', () => {
    expect(isCity('Vienna')).toBeTruthy();
  });

  test('city database has Seoul', () => {
    expect(isCity('Seoul')).toBeTruthy();
  });
});

describe('One-Time Setup - San Juan', () => {
  test('city database has San Juan', () => {
    expect(isCity('San Juan')).toBeTruthy();
  });
});
```

- 매번 반복되는 작업이므로 jest.setup.js 파일을 생성해서 세팅값을 붙여넣으면 유지가 된다.

```js
import { server } from "../../src/commons/mocks"; // 임의로 만든 서버

beforeAll(() => server.listen());
afterAll(() => server.close());
```