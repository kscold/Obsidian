- [[타입스크립트(TypeScript)]]의 타입 시스템은 [[interface]] [[속성(Property)]]을 readonly(읽기 전용)으로 지정할 수 있게 해준다. 
- 이를 통해 [[함수형 프로그래밍(Functional Programming)]] 방식이 가능해진다.


## 예시

```ts
function foo(config: {
    readonly bar: number,
    readonly bas: number
}) {
    // ..
}

let config = { bar: 123, bas: 123 };

foo(config); // config가 변경되지 않는다고 확신할 수 있음
```

- 당연히 readonly는 [[interface]]와 [[type]] 정의에서 모두 사용할 수 있다.

```ts
type Foo = {
    readonly bar: number;
    readonly bas: number;
}

// 초기화는 오케이
let foo: Foo = { bar: 123, bas: 456 };

// 변경은 안됨
foo.bar = 456; // 오류: 상수나 읽기 전용 속성은 대입(assignment) 표현식의 좌항이 될 수 없음
```

- [[클래스(class)]]의 [[속성(Property)]]도 readonly로 선언할 수 있다. 
- 이런 속성은 선언 시점에 초기화하거나 아래 보인 것처럼 [[생성자(Constructor)]]에서 초기화할 수 있다.

```ts
class Foo {
    readonly bar = 1; // OK
    readonly baz: string;
    
    constructor() {
        this.baz = "hello"; // OK
    }
}
```

## readonly vs [[const]]

- readonly 와 [[const]]의 공통점은 생성 후에 [[배열(Array)]]을 변경하지 않음을 보장한다.
- [[변수(Variable)]]는 [[const]]를 사용하고 [[속성(Property)]]은 readonly를 사용한다.

```ts
let arr: Number[] = [1, 2, 3, 4];

let readonly_arr: ReadonlyArray<number> = arr;

readonly_arr[0] = 12;	//Error
readonly_arr.push(5);	//Error
readonly_arr.length = 100;	//Error
```
