여러 실행문들을 하나의 기능으로 구현한 것

스위프트의 기본적인 함수의 정의

- 함수를 정의는 func 키워드로 시작
- 함수의 이름, [[매개변수(parameter)]], 반환값의 자료형으로 정의
- 함수의 반환값의 자료형 앞에 -> 사용
- 함수의 반환값 앞에 return 사용 (단, 함수 내의 코드가 한줄인 경우 반환값 앞에 return 생략 가능)
- 함수 호출 시 매개변수가 있다면 반드시 사용해야 함
- 인자레이블을 사용한다면 매개변수 대신 반드시 인자레이블을 사용해야함

함수 호출
- 함수 호출 시 매개변수가 있다면 반드시 매개변수 사용

```swift
func 함수명(매개변수: 매개변수자료형, 매개변수: 매개변수자료형, …) -> 반환값자료형 {
	실행문
	ruturn 반환값 // 함수 내의 코드가 한 줄인 경우 반환값 앞에 return 생략 가능
}

// 함수 호출
함수명(매개변수: 인자, 매개변수: 인자, …)
```


1. 기본적인 함수 정의 및 호출
```swift
// 함수 호출 시 매개변수(parameter)가 있다면 반드시 사용
func hello(name: String) -> String{
	return "Hello \(name)"
}

func showMyName(name: String) -> String{
	"My name is \(name)." // 함수 내부 코드가 한 줄일 경우 return 생략 가능
}

let helloJenny: String = hello(name: "Jenny") 
// 함수 호출 시 매개변수가 있다면 반드시 매개변수 사용

print(helloJenny)
print(showMyName(name: "Jenny"))
```

2. 함수의 매개변수(parameter)가 없는 경우
```swift
func helloWorld() -> String{ // return 타입만 명시하고 있음
	return "Hello, World!"
}
print(helloWorld()) // 그냥 호출하면 됨
```

스위프트 함수의 특징으로는 인자 레이블(argument label)이 있음
인자 레이블: 매개변수 앞에 사용되며 매개변수의 기능 설명
->  함수에 인자를 넣을 때 가독성을 높이기 위한 것으로 이것도 매개변수의 별명이라고 보면 됨

중요 ! 인자 레이블을 사용하는 함수 호출 시 매개변수 대신 반드시 인자레이블 사용

함수 정의
```swift
func 함수명(인자레이블 매개변수: 매개변수자료형, 인자레이블 매개변수: 매개변수자료형, …) -> 반환값자료형 
{
	실행문
	return 반환값 // 함수 내의 코드가 한 줄인 경우 반환값 앞에 return 생략 가능
}

// 함수 호출
함수명(인자레이블: 인자, 인자레이블: 인자, …) 
```


인자 레이블 사용 시 함수 호출 시 매개변수 대신 인자 레이블 사용
```swift
func 함수명( _ 매개변수: 매개변수자료형, _ 매개변수: 매개변수자료형, …) -> 반환값자료형 
{
	실행문
	return 반환값 // 함수 내의 코드가 한 줄인 경우 반환값 앞에 return 생략 가능
}

// 함수 호출
함수명(인자, 인자, …) // 인자 레이블 사용 시 함수 호출 시 매개변수 대신 인자 레이블 사용
```


1. 인자 레이블(argument label) 사용
```swift
func sayhello(from myName: String, to name: String) -> String{
return "Hello \(name)! I'm \(myName)."
}

print(sayhello(from: "Lee", to: "Kim")) // 인자 레이블 사용할 경우 함수 호출 시 매개변수 대신 인자 레이블 사용
print(sayHello(myName: "Lee", name: "Kim")) 
// 인자 레이블 사용할 경우 함수 호출 시 매개변수 사용하면 에러 발생, 인자레이블이 있을 때는 무조건 인자 레이블로 호출해야함
```

2. argument label이 와일드카드 식별자(`_`)인 경우
```swift
func hello( _ myName: String, _ name: String) -> String{
	return "Hello \(name)! I'm \(myName)."
}

print(hello("Lee", "Kim")) // 함수 호출 시 인자 레이블과 매개변수 생략
print(hello(myName: "Lee", name: "Kim")) 
// 함수 호출 시 매개변수 사용하면 에러 발생, 필요없는데 사용했기 때문에
```