- [[클래스(class)]]에 직접 속하는 [[메서드(Method)]]이다.
- [[인스턴스(instance)]]를 생성하지 않고도 호출할 수 있는 메서드이다.
- 즉, [[this]] 키워드를 사용할 수 없다.

- [[클래스(class)]] 이름을 통해 직접 호출할 수 있다.


## 문법

```ts
class Example {
  static staticMethod() {
    return "이것은 정적 메서드입니다"
  }
}

// 호출 방법
Example.staticMethod()
```

- [[클래스(class)]]의 [[인스턴스(instance)]] 속성에 접근할 수 없다
- 다른 [[정적 메서드(Static method)]]나 정적 [[속성(Property)]]에는 접근 가능하다


## 예시

```ts
class MathOperations {
	static add(x: number, y: number) {
	    return x + y
	}
	
	static multiply(x: number, y: number) {
	    return x * y
	}
}

// 사용 방법
const sum = MathOperations.add(5, 3) // 8
const product = MathOperations.multiply(4, 2) // 8
```

- 유틸리티(Utility) [[함수(Function)]]를 그룹화할 때 자주 사용된다.
- 팩토리 메서드(Factory Method 패턴에서 자주 사용된다.
