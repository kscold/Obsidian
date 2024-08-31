- public은 어디에서나 접근할 수 있으며 생략 가능한 default 값이다.

```ts
class Hello {
	name: string
	
	constructor(name: string) {
		this.name = name
	}
	
	// public 생략 가능
	public greet() {
		console.log(`hi! ${this.name}`)
	}
}

const hello = new Hello('kmj')
hello.greet() // output: 'hi! kmj'
```

- name을 선언해주기 위해서 꽤나 많은 양의 코드를 작성해야 하는데, 이를 constructor에서 한 번에 선언할 수 있다.

```ts
class Hello {
	constructor(public name: string) {}
	// 생략
}
```