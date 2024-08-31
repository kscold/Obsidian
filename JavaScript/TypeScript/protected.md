- protected는 해당 [[클래스(class)]] 혹은 서브 클래스(자식클래스)의 [[인스턴스(Instance)]]에서만 접근이 가능하다.

```ts
// 1. 해당 클래스에서 접근
class Hello {
	constructor(public name: string) {}
	
	greet() {
		console.log(`hi! ${this.name}, log: ${this.test()}`)
	}
	
	protected test() {
		return 'test'
	}
}

const hello = new Hello('kmj')
hello.greet() // output: 'hi! kmj, log: test'

// 2. 서브클래스에서 접근
class Hi extends Hello {}

const hi = new Hi('howdy')
hi.greet() // output: 'hi howdy, log: test'
```

- 단, 서브클래스에서 [[protected]]로 된 값을 [[public]]으로 오버라이딩한다면 해당 값은 [[public]]으로 취급된다.

```ts
class Hi extends Hello {
	test() {
		return 'override'
	}
}

const hi = new Hi('howdy')
hi.greet() // output: 'hi howdy, log: override'

const test = hi.test()
console.log(test) // output: 'override'
```

- 오버라이딩할 경우, 상위클래스의 return 타입과 같아야 한다 .
- 그렇지 않으면 에러를 반환한다.