- Union 타입(합집합 타입)은 [[타입스크립트(TypeScript)]]에서 `A | B` 형태로 작성하며, 변수나 매개변수가 여러 타입 중 하나가 될 수 있음을 표현한다.
- 파이프(`|`) 기호로 두 개 이상의 타입을 나열하면, 나열된 타입 중 하나이기만 하면 타입 검사를 통과한다.
- [[Intersection]] 타입(`A & B`)이 두 타입의 교집합(모두 만족)인 것과 달리, Union은 합집합(하나만 만족)이다.
- [[Interface]]나 [[type]] 별칭, 원시 타입, 리터럴 타입 등 모든 타입을 조합할 수 있다.

## 기본 사용법

```ts
// 원시 타입 Union
let value: string | number;
value = '안녕하세요'; // 가능
value = 42;          // 가능
// value = true;     // 에러 — boolean은 포함되지 않음

// 함수 매개변수에 Union 적용
function printId(id: number | string) {
  console.log(`ID: ${id}`);
}
```

## 리터럴 Union — 열거형 대체

```ts
type Direction = 'left' | 'right' | 'up' | 'down';
type Status = 'pending' | 'success' | 'error';

function move(dir: Direction) {
  console.log(`이동 방향: ${dir}`);
}

move('left');  // 가능
// move('diagonal'); // 에러
```

## 객체 타입 Union

```ts
interface Cat {
  meow(): void;
}

interface Dog {
  bark(): void;
}

type Pet = Cat | Dog;

// 타입 가드로 좁히기 (narrowing)
function makeSound(pet: Pet) {
  if ('meow' in pet) {
    pet.meow();
  } else {
    pet.bark();
  }
}
```

## 타입 가드 (Type Guard)

- Union 타입은 공통되지 않은 속성에 바로 접근할 수 없어서 타입을 좁히는 타입 가드가 필요하다.

```ts
function formatValue(val: string | number): string {
  if (typeof val === 'string') {
    return val.toUpperCase(); // string 메서드 사용 가능
  }
  return val.toFixed(2);     // number 메서드 사용 가능
}
```
