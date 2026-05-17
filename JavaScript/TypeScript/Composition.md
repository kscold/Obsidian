- 컴포지션(Composition, 조합)은 [[상속(Inheritance)]] 대신 독립적인 기능 단위를 조합하여 복잡한 객체를 만드는 설계 패턴이다.
- "상속보다 조합을 선호하라(Favor Composition over Inheritance)"는 객체지향 설계 원칙에서 비롯된 개념이다.
- [[클래스(class)]]가 다른 클래스를 확장(`extends`)하는 대신, 기능을 담당하는 객체를 속성으로 주입받아 위임(delegation)한다.
- [[Interface]]를 통해 의존성을 추상화하면 테스트와 교체가 쉬워진다.

## 상속 방식의 문제점

```ts
// 상속 방식 — 부모 변경이 자식에 영향을 줌
class Animal {
  move() { console.log('이동'); }
}

class Bird extends Animal {
  fly() { console.log('날기'); }
}

class Dog extends Animal {
  swim() { console.log('수영'); }
}

// FlyingFish는 날기도 하고 수영도 해야 하는데
// 단일 상속만 가능하므로 표현이 어렵다
```

## 컴포지션 방식

```ts
// 기능을 독립 인터페이스로 분리
interface Flyable {
  fly(): void;
}

interface Swimmable {
  swim(): void;
}

// 기능 구현체
class FlyBehavior implements Flyable {
  fly() { console.log('날개로 날기'); }
}

class SwimBehavior implements Swimmable {
  swim() { console.log('지느러미로 수영'); }
}

// 조합으로 FlyingFish 구성
class FlyingFish {
  private flyer: Flyable;
  private swimmer: Swimmable;

  constructor() {
    this.flyer = new FlyBehavior();
    this.swimmer = new SwimBehavior();
  }

  fly() { this.flyer.fly(); }
  swim() { this.swimmer.swim(); }
}

const fish = new FlyingFish();
fish.fly();   // 날개로 날기
fish.swim();  // 지느러미로 수영
```

## 의존성 주입(DI)과 컴포지션

- 외부에서 구현체를 주입받으면 테스트와 교체가 용이하다.

```ts
interface Logger {
  log(message: string): void;
}

class ConsoleLogger implements Logger {
  log(message: string) { console.log(`[LOG] ${message}`); }
}

class UserService {
  constructor(private logger: Logger) {}

  createUser(name: string) {
    // 로직 수행
    this.logger.log(`사용자 생성: ${name}`);
  }
}

const service = new UserService(new ConsoleLogger());
service.createUser('홍길동');
```

## [[상속(Inheritance)]]과의 비교

| 구분 | 상속 | 컴포지션 |
|------|------|---------|
| 결합도 | 강결합 | 약결합 |
| 유연성 | 낮음 | 높음 |
| 재사용 단위 | 클래스 전체 | 기능 단위 |
| 테스트 | 어려움 | 쉬움 |
