- [[객체(Object)]]의 [[속성(Property)]]값이 함수인 것이다.
- 어떤 객체의 속성으로 접근해서 사용하는 함수를 method(메서드)라고 부른다.
- 메소드는 어떠한 작업을 수행하는 코드를 하나로 묶어 놓은 것이다. 

따라서 .concat(), find() 같은 함수들을 매소드라고 부른다.

메소드 내의 변수는 지역변수로, 메소드 내부에서만 사용할 수 있다. 따라서 .로 접근한다.

자바 스크립트에서 메서드는 크게 2개로 나눌 수 있다.

1. 일반 메서드 (Instance Method)
    - 일반 메소드는 [[클래스(class)]]로부터 생성된 개별 객체(인스턴스)에 속하는 메서드입니다.
    - 인스턴스 메소드는 해당 클래스의 모든 인스턴스에 대해 동일한 동작을 수행하지만, 호출될 때마다 메소드 내부에서 [[this]] 키워드는 호출된 [[객체(Object)]]([[인스턴스(Instance)]])를 참조합니다.
    - 예를 들어, `getUserInfo()` 메소드가 인스턴스 메소드인 경우, `user1.getUserInfo()`와 `user2.getUserInfo()`는 각각 `user1`과 `user2` 객체에 대해 다른 정보를 반환할 수 있습니다.


1. 정적 메소드 (Static Method)
    - 정적 메소드는 클래스 자체에 속하는 메소드로, 클래스의 인스턴스를 생성하지 않고 직접 클래스 이름을 통해 호출됩니다.
    - 정적 메소드는 일반적으로 클래스와 관련된 유틸리티 함수를 제공하거나, 특정 클래스의 속성을 조작하는 데 사용됩니다.
    - 정적 메소드 내에서는 [[this]] 키워드가 클래스 자체를 참조하며, 객체의 인스턴스에 접근할 수 없습니다.

쉽게 말해서 일반 메소드는 객체의 인스턴스를 가리키고 정적 메소드는 객체  혹은 스키마 그자체를 가리킨다.

### 예시

```js
class MyClass {
  constructor(data) {
    this.data = data;
  }

  // 인스턴스 메소드
  getInstanceData() {
    return this.data;
  }

  // 정적 메소드
  static staticMethod() {
    return 'This is a static method.';
  }
}

const instance = new MyClass('Hello');

console.log(instance.getInstanceData()); // "Hello"
console.log(MyClass.staticMethod()); // "This is a static method."
``` 

정적 메소드는 MyClass 그 자체를 가리킴 일반 메소드는 인스턴스인 instance를 가리킴