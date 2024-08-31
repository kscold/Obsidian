- private는 해당 [[클래스(class)]]의 [[인스턴스(Instance)]]에서만 접근 가능하다.

```ts
class Hello {
	constructor(private name: string) {}
}

const hello = new Hello('kmj')
hello.name // Property 'name' is private and only accessible within class 'Hello'.
```

- 위의 예시에서 name을 가져오려하려면, 위와 같은 에러가 뜬다.
- 그리고 서브클래스(자식 클래스)에서 name을 [[public]]으로 바꾸어주려고 해도 에러가 뜬다.

```ts
class Hi extends Hello {
	constructor(public name: string) {
		super(name)
	}
	// Class 'Hi' incorrectly extends base class 'Hello'.
	// Property 'name' is private in type 'Hello' but not in type 'Hi'.ts(2415)
}
```