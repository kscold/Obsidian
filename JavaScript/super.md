- `super` 키워드는 객체 리터럴 또는 클래스의 [[프로토타입(Prototype)]] 속성에 접근하거나 슈퍼클래스의 생성자를 호출하는 데 사용됩니다.

`super.prop`와 `super[expr]` 표현식은 클래스와 객체 리터럴의 메서드 정의에서 모두 사용할 수 있습니다. `super(...args)` 표현식은 클래스 생성자에서 유효합니다.


```js
class Foo {
  constructor(name) {
    this.name = name;
  }

  getNameSeparator() {
    return '-';
  }
}

class FooBar extends Foo {
  constructor(name, index) {
    super(name);
    this.index = index;
  }

  getFullName() {
    return this.name + super.getNameSeparator() + this.index;
  }
}

const firstFooBar = new FooBar('foo', 1);

console.log(firstFooBar.name);
// Expected output: "foo"

console.log(firstFooBar.getFullName());
// Expected output: "foo-1"
```

```js
super([arguments]) // 부모 생성자 호출.
super.propertyOnParent
super[expression]
```

### 설명

`super` 키워드는 "함수 호출"(`super(...args)`) 또는 "속성 조회"(`super.prop`와 `super[expr]`)의 두 가지 방법으로 사용할 수 있습니다.