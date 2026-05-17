- `new` 키워드는 [[생성자 함수(Constructor Function)]] 또는 [[클래스(class)]]를 호출해서 새로운 [[인스턴스(Instance)]]를 만드는 연산자이다.
- `new` 없이 [[생성자(Constructor)]]를 호출하면 `this`가 전역 객체를 가리키므로 반드시 `new`를 붙여야 한다.

## new 연산자가 하는 4단계 동작

1. **빈 객체 생성** — `{}` 형태의 새 빈 [[객체(Object)]]를 메모리에 만든다.
2. **this 바인딩** — 생성된 빈 객체를 `this`에 [[바인딩(binding)]]한다.
3. **프로퍼티 추가** — 생성자 함수 본문을 실행하며 `this`에 [[속성(Property)]]과 [[메서드(Method)]]를 추가한다.
4. **반환** — 생성자 함수가 명시적으로 객체를 `return`하지 않으면 `this`(새 객체)를 자동으로 반환한다.

## 기본 사용법

```js
function Person(name, age) {
  this.name = name; // 2단계: this에 바인딩된 객체에 프로퍼티 추가
  this.age = age;
}

const alice = new Person('Alice', 30); // 4단계: this(새 객체)가 반환됨
console.log(alice.name); // >> Alice
console.log(alice instanceof Person); // >> true
```

## 클래스와 함께 사용

- ES6 [[클래스(class)]] 문법에서도 동일하게 `new`로 [[인스턴스(Instance)]]를 생성한다.

```js
class Animal {
  constructor(type) {
    this.type = type;
  }
  speak() {
    return `${this.type} says hello`;
  }
}

const dog = new Animal('Dog');
console.log(dog.speak()); // >> Dog says hello
```

## prototype 연결

- `new`로 생성된 객체의 `__proto__`는 자동으로 생성자 함수의 [[prototype]]에 연결된다.

```js
function Cat() {}
Cat.prototype.meow = function () { return 'meow'; };

const kitty = new Cat();
console.log(kitty.meow()); // >> meow (프로토타입 체인으로 접근)
```

## new 없이 호출했을 때의 문제

```js
function Person(name) {
  this.name = name;
}

const p = Person('Bob'); // new 없이 호출 → this가 전역 객체
console.log(p);          // >> undefined
console.log(globalThis.name); // >> Bob (전역이 오염됨)
```

- `new.target`을 사용하면 `new` 없이 호출됐는지 감지할 수 있다.

```js
function Safe(name) {
  if (!new.target) throw new Error('new 키워드를 사용하세요.');
  this.name = name;
}
```
