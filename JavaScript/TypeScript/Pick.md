- `Pick<T, K>`은 [[타입스크립트(TypeScript)]]의 유틸리티 타입으로, 기존 타입 `T`에서 특정 키 `K`만 골라 새로운 타입을 만든다.
- 불필요한 속성을 제거하고 필요한 필드만 추출할 때 사용한다.
- `K`는 `T`의 키 중 하나 혹은 여러 개를 [[Union]] 타입으로 지정할 수 있다.
- [[Omit]]과 반대 개념으로, Pick은 "남길 것"을 지정하고 Omit은 "제거할 것"을 지정한다.
- [[Interface]]나 [[type]]으로 정의된 타입 모두에 적용할 수 있다.

## 기본 사용법

```ts
interface User {
  id: number;
  name: string;
  email: string;
  password: string;
}

// id와 name만 추출한 새 타입
type UserPreview = Pick<User, 'id' | 'name'>;

const preview: UserPreview = {
  id: 1,
  name: '홍길동',
  // email, password는 포함 불가 — 컴파일 에러 발생
};
```

## 함수 매개변수에서의 활용

```ts
interface Product {
  id: number;
  title: string;
  price: number;
  stock: number;
  description: string;
}

// 목록 조회 시 일부 필드만 반환
function getProductSummary(product: Pick<Product, 'id' | 'title' | 'price'>) {
  return product;
}
```

## 내부 구현 원리

- `Pick`은 [[Mapped Type]]을 활용해 아래처럼 직접 구현할 수도 있다.

```ts
type MyPick<T, K extends keyof T> = {
  [P in K]: T[P];
};
```

- `K extends keyof T` 제약 덕분에 존재하지 않는 키를 지정하면 컴파일 에러가 발생한다.
