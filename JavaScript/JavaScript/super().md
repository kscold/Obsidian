- `super` [[키워드(Keyword)]]는 객체 [[리터럴(literal)]] 또는 [[클래스(Class)]]의 [[prototype]] 속성에 접근하거나 슈퍼클래스(부모 클래스)의 [[생성자(Constructor)]]를 호출하는 데 사용된다.

`super.prop`와 `super[expr]` 표현식은 클래스와 객체 리터럴의 [[메서드(Method)]] 정의에서 모두 사용할 수 있다. `super(...args)` 표현식은 클래스 생성자에서 유효하다.


```js
class Foo { // 부모 클래스(슈퍼 클래스) 선언
  constructor(name) { // 생성자 선언
    this.name = name;
  }

  getNameSeparator() {
    return '-';
  }
}

class FooBar extends Foo { // 클래스를 상속( 자식클래스)
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

`super` 키워드는 "함수 호출"(`super(...args)`) 또는 "속성 조회"(`super.prop`와 `super[expr]`)의 두 가지 방법으로 사용할 수 있다.