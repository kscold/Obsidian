- [[TypeScript]]에서 `enum`은 관련된 상수들의 집합을 이름으로 묶어 관리하는 열거형 타입이다.
- 숫자형과 문자열형 두 가지가 있으며, 코드의 의도를 명확하게 표현할 때 사용한다.
- enum은 그 자체로 [[객체(Object)]]이기도 하다.
- 그래서 `Object.keys` 를 사용하면 key값이 배열에 담겨 나오기도 한다.
- 하지만 차이점도 존재한다.

## 숫자형 Enum (Numeric Enum)

- 기본값은 0부터 시작하며 자동으로 증가한다.
- 직접 초기값을 지정할 수 있고, 이후 멤버는 이전 값 +1로 이어진다.

```ts
enum Direction {
  Up,    // 0
  Down,  // 1
  Left,  // 2
  Right, // 3
}

enum Status {
  Pending = 1,
  Active = 2,
  Inactive = 4,
}

console.log(Direction.Up);      // 0
console.log(Direction[0]);      // 'Up' — 역방향 매핑 가능
```

- 숫자형 enum은 **역방향 매핑(Reverse Mapping)**을 지원한다. `Direction[0]`처럼 값으로 이름을 조회할 수 있다.

## 문자열형 Enum (String Enum)

- 각 멤버에 문자열 리터럴을 직접 할당한다.
- 역방향 매핑을 지원하지 않지만, 디버깅 시 값이 의미 있는 문자열로 출력되어 실용적이다.

```ts
enum Color {
  Red = 'RED',
  Green = 'GREEN',
  Blue = 'BLUE',
}

const c: Color = Color.Green;
console.log(c); // 'GREEN'
```

## const Enum

- `const enum`은 컴파일 시 인라인으로 치환되어 런타임 객체를 생성하지 않는다. 번들 크기를 줄이는 데 유용하다.

```ts
const enum Direction {
  Up = 'UP',
  Down = 'DOWN',
}

const move = Direction.Up;
// 컴파일 결과: const move = 'UP'; — 런타임에 Direction 객체 없음
```

## enum vs 유니온 타입(Union Type)

- 최신 TypeScript에서는 enum 대신 `as const`와 유니온 타입을 사용하는 패턴도 많이 쓰인다.

```ts
// enum 방식
enum Role { Admin = 'ADMIN', User = 'USER' }

// as const + 유니온 방식 (Tree-shakeable, 런타임 객체 없음)
const Role = { Admin: 'ADMIN', User: 'USER' } as const;
type Role = typeof Role[keyof typeof Role]; // 'ADMIN' | 'USER'
```

## [[객체(Object)]]와 enum의 차이점

- `object` 는 코드내에서 새로운 속성을 자유롭게 추가가 가능하나 `enum`은 선언 이후 변경할 수 없다.
- `object`의 속성값은 JS의 모든 타입이 올 수 있지만, `enum`은 `string` 혹은 `number` 만 가능하다.

## 이종 Enum (Heterogeneous Enum)

- 숫자와 문자열을 혼합하여 사용하는 방식이다. 권장되지는 않지만 문법상 허용된다.

```ts
enum Mixed {
  No = 0,
  Yes = 'YES',
}
```

## enum을 타입으로 활용하기

- enum은 타입으로도 사용할 수 있어 함수 파라미터나 변수의 타입을 제한하는 데 쓰인다.

```ts
enum Direction {
  Up = 'UP',
  Down = 'DOWN',
}

function move(dir: Direction): void {
  console.log(`Moving ${dir}`);
}

move(Direction.Up);   // OK
move('UP');           // 오류: string은 Direction에 할당 불가
```

## enum 값 순회

- `Object.values()`나 `Object.keys()`를 이용해 enum의 멤버를 순회할 수 있다.
- 숫자형 enum은 역방향 매핑 키도 포함되므로 주의가 필요하다.

```ts
enum Color {
  Red = 'RED',
  Green = 'GREEN',
  Blue = 'BLUE',
}

// 문자열 enum은 순회가 깔끔하다
Object.values(Color).forEach((v) => console.log(v));
// 'RED', 'GREEN', 'BLUE'

enum Num {
  A = 0,
  B = 1,
}

// 숫자형 enum은 역방향 키도 포함됨
console.log(Object.keys(Num)); // ['0', '1', 'A', 'B']
```

## NestJS에서 enum 활용 패턴

- [[NestJS]]에서는 도메인 상수를 enum으로 관리하고, DTO의 `@ApiProperty`와 연계하여 Swagger 문서를 자동화하는 패턴을 사용한다.

```ts
// user.enum.ts
export enum UserRole {
  Adopter = 'adopter',
  Breeder = 'breeder',
  Admin = 'admin',
}

// DTO에서 활용
import { ApiProperty } from '@nestjs/swagger';
import { IsEnum } from 'class-validator';

export class CreateUserDto {
  @ApiProperty({ enum: UserRole, example: UserRole.Adopter })
  @IsEnum(UserRole)
  role: UserRole;
}
```
