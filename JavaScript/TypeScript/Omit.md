- `Omit<T, K>`은 [[타입스크립트(TypeScript)]]의 유틸리티 타입으로, 기존 타입 `T`에서 특정 키 `K`를 제거한 새로운 타입을 만든다.
- [[Pick]]과 반대 개념으로, Pick은 "남길 것"을 지정하고 Omit은 "제거할 것"을 지정한다.
- `K`는 [[Union]] 타입으로 여러 키를 동시에 제거할 수 있다.
- [[Interface]]나 [[type]] 모두에 적용 가능하며, 응답 DTO에서 민감 필드를 제거할 때 자주 사용한다.

## 기본 사용법

```ts
interface User {
  id: number;
  name: string;
  email: string;
  password: string; // 민감 정보
}

// password 필드를 제거한 안전한 타입
type PublicUser = Omit<User, 'password'>;
// { id: number; name: string; email: string; }

const publicUser: PublicUser = {
  id: 1,
  name: '홍길동',
  email: 'hong@example.com',
  // password는 포함할 수 없음
};
```

## 여러 키 동시 제거

```ts
interface Article {
  id: number;
  title: string;
  content: string;
  authorId: string;
  createdAt: Date;
  updatedAt: Date;
}

// 메타 정보 필드들을 제거하고 입력 폼 타입으로 사용
type ArticleCreateForm = Omit<Article, 'id' | 'createdAt' | 'updatedAt'>;
```

## Pick과의 비교

```ts
interface Product {
  id: number;
  name: string;
  price: number;
  stock: number;
}

// 아래 두 타입은 동일한 결과를 만든다
type ProductSummaryWithPick = Pick<Product, 'id' | 'name' | 'price'>;
type ProductSummaryWithOmit = Omit<Product, 'stock'>;
// 제거할 키가 적을 때 Omit, 남길 키가 적을 때 Pick이 더 간결
```

## 내부 구현 원리

```ts
// Omit은 내부적으로 Pick과 Exclude를 조합해 구현됨
type MyOmit<T, K extends keyof T> = Pick<T, Exclude<keyof T, K>>;
```
