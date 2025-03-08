- 같은 이름의 [[메서드(Method)]]를 매개변수의 타입이나 개수를 다르게 하여 여러 번 정의하는 것이다.

- 자바스크립트는 기본적으로 오버로딩을 지원하지 않고, [[타입스크립트(TypeScript)]]에서만 하나의 [[함수(Function)]]나 [[메서드(Method)]]가 여러 개의 호출 시그니처를 가질 수 있게 한다.

- 실제 구현부는 모든 오버로드된 시그니처를 처리할 수 있어야 한다.


## 문법

```ts
class Calculator {
	// 숫자 두 개를 더하는 메서드
	add(x: number, y: number): number
	// 문자열 두 개를 연결하는 메서드
	add(x: string, y: string): string
	// 실제 구현
	add(x: any, y: any): any {
	    return x + y
	}
}
```

- 같은 이름의 [[메서드(Method)]]를 여러 개 정의할 수 있다.
- 매개변수의 타입이나 개수가 달라야 한다.
- [[재정의(Override)]]와는 다른 개념이다.


## 예시

```ts
class StringOrNumber {
	// 숫자를 받는 경우
	process(value: number): number
	
	// 문자열을 받는 경우
	process(value: string): string
	
	// 실제 구현
	process(value: any): any {
	    if (typeof value === "number") {
		    return value * 2
		}
		
		return value.toUpperCase()
	}
}

const processor = new StringOrNumber()
console.log(processor.process(10)) // 20
console.log(processor.process("hello")) // "HELLO"
```