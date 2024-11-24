- [[타입 표기(Type Annotation)]]을 직접적으로 고정된 값으로 명시하지말고 [[변수(Variable)]]를 통해 언제든지 변할 수 있는 타입을 통해 보다 유연하게 코딩을 할 수 있는 장치가 필요한데 이것 제네릭(generic) 타입이다.

- 즉, 타입을 [[변수(Variable)]]화 한 것이라고 할 수 있다.


## 제네릭을 [[클래스(class)]]에 사용

- 아래는 간단하게 Stack을 구현한 클래스이다.

```ts
class Stack {
    private stack: any[] = [];
	
    constructor() {}
	
    push(item: any): void {
        this.stack.push(item)
    }
	
    pop(): any{
        return this.stack.pop();
    }
}
```

- 자료구조는 보통 범용적으로 타입을 수용할 수 있더록 만들어야 한다.
- 그래서 [[타입스크립트(TypeScript)]]는 any를 이용하여 구현할 수 있는게 가장 쉽다.

- 하지만 문제는 아래에서 발생한다.

```ts
const stack = new Stack();

stack.push(1);
stack.push('a');
stack.pop().substring(0); // 'a'
stack.pop().substring(0); // Type Error
```

- 그러면 타입에 딱 맞게 스택들을 만드려면 다음과 같이 구현을 해야한다.

```ts
class NumberStack extends Stack {
    constructor() {
        super();
    }
	
    push(item: number) {
        super.push(item);
    }
    
    pop(): number {
        return super.pop();
    }
}
```

- 하지만 이렇게 되면 문제점은 새로운 자료형을 추가할 때 마다 [[클래스(class)]]를 계속 추가해줘야 한다.
- 따라서 이런 경우에 제네릭을 사용한다.

```ts
class Stack<T> {
    private stack: T[] = [];
	
    constructor() {}
	
    push(item: T): void {
        this.stack.push(item)
    }
	
    pop(): T{
        return this.stack.pop();
    }
}
```

- T는 Type의 약자로 다른 언어에서도 제네릭을 선언할 때 많이 사용 된다.
- 여기서 T를 [[타입 변수(Type Variable)]]라고 한다.

- 이렇게해서 [[클래스(class)]]에서 제네릭을 사용하겠다고 선언한 경우 T는 해당 [[클래스(class)]]에서 사용할 수 있는 특정한 타입이 된다.

```ts
const numStack = new Stack<number>();
numStack.push(1);

const strStack = new Stack<string>();
strStack.push('a')
```

- 이렇게 [[클래스(class)]] 를 선언하면 하나의 [[클래스(class)]]를 통해 여러가지 타입을 다룰 수 있다.



## 제네릭을 [[함수(Function)]]에 사용

- [[배열(Array)]]을 받아서 [[배열(Array)]]의 첫번째 요소를 반환하는 [[함수(Function)]]를 만든다고 가정을 했을 때 제네릭을 안쓰고 구현 하면 다음과 같이 될 것이다.

```ts
function first (arr: any[]): any{
    return arr[0]
}
```

- 위의 방식은 [[클래스(class)]]와 했던 방식과 마찬가지로 모든 타입의 [[배열(Array)]]을 받기 때문에 리턴하게 되는 것이 무슨 타입인지 알 수 없다.

- 이것을 해결하기 위해서 아래와 같이 제네릭을 쓰면 된다.

```ts
function first<T> (arr: T[]): T{
	return arr[0]
}
```

- [[클래스(class)]]에서 했던 방식과 동일하게 함수명 오른쪽에 `<T>` [[키워드(Keyword)]]를 붙혀주고 특정 타입을 T로 지정해준다.

- 사용할 때는 다음과 같이 사용한다.

```ts
const number = first<number>([1,2,3]) // 1
```


## 두 개 이상의 타입 변수

- 제네릭 [[함수(Function)]]나 [[클래스(class)]]에서는 두개 이상의 [[타입 변수(Type Variable)]]도 사용 할 수 있다.

```ts
function toPair(a: any, b: any): [ any, any]{
    return [a ,b];
}
```

- 위의 코드에서는 두개의 변수 타입이 다르다고 가정하고 두가지 [[타입 변수(Type Variable)]]가 필요하게 된다.

- 이를 제네릭으로 바꾸면 아래와 같은 형식으로 수정 할 수 있다.

```ts
function toPair<T, U>(a: T, b: U): [ T, U ] {
    return [a, b]
}
```

- 원래는 `<T>` 만 사용하였다면 `<T, U>` 두 가지의 [[타입 변수(Type Variable)]]로 선언 할 수 있다.
- 해당 [[함수(Function)]]를 사용하는 방법은 다음과 같이 사용하면 된다.

```ts
toPair<string, number>('1', 1);
toPair<number, number>(1, 1);
```


## 인터페이스([[Interface]])와 제네릭

- [[타입 변수(Type Variable)]]는 기존에 사용하고 있는 타입을 상속할 수도 있다.
- 이 점을 이용하면 입력 받을 [[변수(Variable)]]의 [[타입 표기(Type Annotation)]]를 제한할 수 있다.

- 예를 들어 특정 인터페이스([[Interface]]) 혹은 타입을 통해 [[타입 표기(Type Annotation)]]를 제한 할 수도 있다.

```ts
interface Item {
    name: string;
    price: number;
    stock: number;
}

function getItem<T extends Item> (item: T): T{
    return item;
}
```

- 위와 같은 코드가 있으면 제네릭으로 받는 타입 T는 Item의 하위 타입이다.
- T는 항상 name, price, stock이 모두 포함 되어야 한다.

```ts
function getItem<T extends keyof Item> (item: T): T{
    return item;
}
```

- keyof 타입을 사용하면 name, price, stock 중에 하나만 들어가면 된다.