- [[클래스(class)]]에서 부모 클래스(Parent Class)의 [[메서드(Method)]]를 자식 클래스(Child Class)에서 새롭게 정의하는 것이다.
- [[상속(Inheritance)]] 관계에서 사용되는 객체지향 프로그래밍의 중요한 특징이다.

- [[타입스크립트(TypeScript)]]에서는 'override' 키워드를 사용하여 명시적으로 표시할 수 있다.
- [[메서드(Method)]]의 이름은 같지만 동작을 다르게 구현할 수 있다.

- 다형성(Polymorphism)을 구현하는 방법 중 하나이다.

- [[중복 정의(Overloading)]]과는 다른 개념이다.

- 부모 [[클래스(class)]](Parent Class)의 메서드와 동일한 시그니처를 가져야 한다.
- [[super()]] [[키워드(Keyword)]]를 통해 부모 [[메서드(Method)]]의 기능을 호출할 수 있다.


## 문법

```ts
class Animal {
    makeSound() {
        return "Some sound";
    }
}

class Dog extends Animal {
    override makeSound() {
        return "Woof!";
    }
}
```


## 예시

```ts
class ConfigService {
    get(key: string) {
        return process.env[key];
    }
}

class CustomConfigService extends ConfigService {
    override get(key: string) {
        const value = super.get(key);
        return value ? value.toUpperCase() : null;
    }
}
```


