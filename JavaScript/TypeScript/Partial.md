- `Partial<T>`은 [[타입스크립트(TypeScript)]]의 유틸리티 타입으로, 타입 `T`의 모든 속성을 [[선택적 속성(Optional Properties)]](`?`)으로 만든 새 타입을 반환한다.
- 기존 [[Interface]]나 [[type]]의 일부 필드만 업데이트할 때 자주 사용한다.
- 반대 개념은 `Required<T>`로, 모든 선택적 속성을 필수로 바꾼다.

## 기본 사용법

```ts
interface User {
  id: number;
  name: string;
  email: string;
}

// 모든 필드가 optional이 된 타입
type PartialUser = Partial<User>;
// { id?: number; name?: string; email?: string; }

const update: PartialUser = {
  name: '홍길동', // id, email 없어도 에러 없음
};
```

## PATCH 요청 DTO 패턴

- REST API의 PATCH(부분 수정) 요청에서 특히 유용하다.

```ts
interface Article {
  id: number;
  title: string;
  content: string;
  author: string;
}

// PATCH 요청 — 변경할 필드만 전달
function updateArticle(id: number, changes: Partial<Omit<Article, 'id'>>) {
  // changes는 title, content, author 중 일부만 포함해도 됨
  console.log('수정 내용:', changes);
}

updateArticle(1, { title: '새 제목' });
```

## 내부 구현 원리

- `Partial`은 [[Mapped Type]]과 `?` 수식어로 아래처럼 직접 구현할 수 있다.

```ts
type MyPartial<T> = {
  [P in keyof T]?: T[P];
};
```

## Object.assign과 함께 불변 업데이트

```ts
interface Config {
  host: string;
  port: number;
  debug: boolean;
}

function mergeConfig(base: Config, overrides: Partial<Config>): Config {
  return { ...base, ...overrides };
}

const defaultConfig: Config = { host: 'localhost', port: 3000, debug: false };
const devConfig = mergeConfig(defaultConfig, { debug: true });
```
