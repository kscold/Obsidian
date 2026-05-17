- `@Type()`은 class-transformer에서 JSON을 [[클래스(class)]] 인스턴스로 변환할 때 중첩 객체의 타입을 명시적으로 지정하는 [[데코레이터(Decorator)]]이다.
- TypeScript는 런타임에 타입 정보를 잃으므로, 중첩 객체 변환 시 반드시 `@Type()`으로 타입을 알려줘야 한다.

## 설치

```bash
pnpm add class-transformer reflect-metadata
```

## 기본 사용법

- `() => TargetClass` 형식의 팩토리 함수를 인자로 받는다.

```ts
import { Type } from 'class-transformer';
import { plainToInstance } from 'class-transformer';

class Address {
  street: string;
  city: string;
}

class User {
  name: string;

  @Type(() => Address) // Address 클래스로 변환하도록 지시
  address: Address;
}

const plain = {
  name: 'Alice',
  address: { street: '강남대로', city: '서울' },
};

const user = plainToInstance(User, plain);
console.log(user.address instanceof Address); // >> true
```

## 배열 중첩 객체에도 적용 가능

```ts
class OrderItem {
  productId: string;
  quantity: number;
}

class Order {
  @Type(() => OrderItem) // OrderItem[] 배열 전체를 변환
  items: OrderItem[];
}

const order = plainToInstance(Order, {
  items: [
    { productId: 'p1', quantity: 2 },
    { productId: 'p2', quantity: 1 },
  ],
});

console.log(order.items[0] instanceof OrderItem); // >> true
```

## NestJS DTO에서의 활용

- NestJS의 `ValidationPipe`와 함께 사용할 때, 중첩 DTO에 `@Type()`을 붙이지 않으면 class-validator의 `@ValidateNested()`가 제대로 동작하지 않는다.

```ts
import { Type } from 'class-transformer';
import { ValidateNested, IsArray } from 'class-validator';

class PetDto {
  @IsString()
  name: string;
}

class CreateBreederDto {
  @IsArray()
  @ValidateNested({ each: true })
  @Type(() => PetDto) // 반드시 필요 — 없으면 검증이 일반 객체로 처리됨
  pets: PetDto[];
}
```

## 숫자 타입 변환

- 쿼리스트링은 항상 문자열로 들어오므로, `@Type(() => Number)`로 숫자 변환을 지시할 수 있다.

```ts
class PaginationDto {
  @Type(() => Number) // '10' → 10 으로 변환
  @IsInt()
  page: number;
}
```
