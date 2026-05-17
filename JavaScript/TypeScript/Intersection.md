- Intersection 타입(교집합 타입)은 [[타입스크립트(TypeScript)]]에서 `A & B` 형태로 작성하며, 두 타입의 속성을 모두 가진 새로운 타입을 만든다.
- `&` 기호로 연결된 모든 타입의 조건을 동시에 만족해야 한다.
- [[Union]] 타입(`A | B`)이 둘 중 하나만 만족하면 되는 것과 달리, Intersection은 모두 만족해야 한다.
- [[Interface]]의 다중 상속처럼 여러 타입의 속성을 합쳐서 재사용할 때 사용한다.
- [[type]] 별칭에서만 사용할 수 있으며, interface는 `extends`로 유사한 효과를 낸다.

## 기본 사용법

```ts
interface Named {
  name: string;
}

interface Aged {
  age: number;
}

// Named와 Aged 두 타입의 속성을 모두 가짐
type Person = Named & Aged;

const person: Person = {
  name: '홍길동', // Named 속성
  age: 30,       // Aged 속성 — 둘 다 필수
};
```

## 믹스인(Mixin) 패턴

```ts
interface Serializable {
  serialize(): string;
}

interface Loggable {
  log(message: string): void;
}

type LoggableEntity = Serializable & Loggable;

class User implements LoggableEntity {
  constructor(public name: string) {}

  serialize() {
    return JSON.stringify({ name: this.name });
  }

  log(message: string) {
    console.log(`[User] ${message}`);
  }
}
```

## 함수 매개변수 확장

```ts
interface BaseRequest {
  userId: string;
}

interface PaginationOptions {
  page: number;
  limit: number;
}

// 두 타입을 합쳐 매개변수로 사용
function getItems(options: BaseRequest & PaginationOptions) {
  const { userId, page, limit } = options;
  console.log(`사용자 ${userId}의 ${page}페이지, ${limit}개 조회`);
}
```

## 원시 타입 Intersection의 주의점

- 원시 타입끼리 Intersection하면 `never`가 될 수 있다.

```ts
type Impossible = string & number; // never — string이면서 number인 값은 없음
```
