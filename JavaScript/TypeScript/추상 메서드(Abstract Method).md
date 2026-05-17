- 추상 메서드(Abstract Method)는 [[추상 클래스(Abstract Class)]] 내부에 선언되지만 구현부(body)가 없는 메서드로, 하위 [[클래스(class)]]가 반드시 재정의(override)해야 한다.
- `abstract` 키워드를 붙여 선언하며, 중괄호 `{}` 없이 시그니처만 작성한다.
- 추상 메서드는 추상 클래스 안에서만 선언할 수 있고, 일반 클래스에는 사용할 수 없다.
- [[Interface]]의 모든 메서드가 암묵적으로 추상 메서드인 것과 달리, 추상 클래스는 일반 메서드와 추상 메서드를 함께 가질 수 있다.

## 기본 선언과 구현

```ts
abstract class Shape {
  // 추상 메서드 — 구현 없음, 하위 클래스 강제 구현
  abstract getArea(): number;

  // 일반 메서드 — 공통 로직 제공
  describe(): string {
    return `이 도형의 넓이는 ${this.getArea()}입니다.`;
  }
}

class Circle extends Shape {
  constructor(private radius: number) { super(); }

  // 추상 메서드 반드시 구현 — 미구현 시 컴파일 에러
  getArea(): number {
    return Math.PI * this.radius ** 2;
  }
}

class Rectangle extends Shape {
  constructor(private width: number, private height: number) { super(); }

  getArea(): number {
    return this.width * this.height;
  }
}

const circle = new Circle(5);
console.log(circle.describe()); // 이 도형의 넓이는 78.53...입니다.
```

## [[Interface]]와의 차이점

```ts
// Interface — 모든 메서드가 추상, 구현 없음, abstract 키워드 불필요
interface Drawable {
  draw(): void;
}

// Abstract Class — 추상/일반 메서드 혼재 가능
abstract class BaseRenderer {
  abstract render(): void;  // 반드시 구현

  init() {                  // 공통 초기화 로직 제공
    console.log('렌더러 초기화');
  }
}
```

## 추상 속성(Abstract Property)

- 메서드뿐 아니라 속성도 추상으로 선언할 수 있다.

```ts
abstract class Animal {
  abstract name: string;       // 추상 속성
  abstract makeSound(): void;  // 추상 메서드

  move() {
    console.log(`${this.name}이(가) 이동합니다.`);
  }
}

class Dog extends Animal {
  name = '강아지'; // 추상 속성 구현

  makeSound() {
    console.log('멍멍!');
  }
}
```

## 사용 지침

- 여러 하위 클래스에 공통 로직을 제공하면서 일부 동작만 강제하고 싶을 때 추상 클래스 + 추상 메서드를 선택한다.
- 공통 로직이 없고 계약(contract)만 정의하면 충분할 때는 [[Interface]]를 선택한다.
