- 특정 자료형을 Optional 형으로 wrapping 한다.
- Optional 형으로 wrapping된 자료형으로 선언된 변수의 값은 있을 수도 없을 수(nil)도 있다.
- 변수의 값이 nil인 경우를 안전하게 처리하기 위해서 사용한다.
- 사용방법: Optional<자료형> 또는 자료형 옆에 느낌표(?) 붙인다.
- Optional은 generice이 적용된 enum형이다.

```swift
var name1: String = "kjsung"
var name2: String = nil // 에러발생 (변수의 값은 nil로 설정 불가능)

var name3: Optional<String> = "kjsung" 
// Optional<자료형> 형으로 선언된 변수의 값은 있을 수 있음
var name4: Optional<String> = nil
// Optional<자료형> 형으로 선언된 변수의 값은 없을 수도 있음 (nil로 설정 가능)

var name5: String? = "kjsung" // Optional<자료형> 형을 자료형?으로 표현 가능
var name6: String? = nil

print(name5)
// 실행 결과
// Optional("kjsung") // Optional<자료형> 형으로 선언된 변수 출력

print(name5!) // forced unwrapping (만약, name5이 nil이면 에러 발생)
// 실행 결과
// kjsung
```

#### Optional Wrapping(옵셔널 래핑)

예를 들어, 정수 값을 나타내는 변수를 선언할 때, 그 변수는 항상 어떤 값을 가지고 있어야 한다.
그러나 옵셔널로 선언한 변수는 값을 가질 수도 있고, `nil`을 가질 수도 있다. 
이렇게 값을 `nil`로 설정하면 해당 변수는 더 이상 유효한 값을 갖지 않는데, 이때 옵셔널은 값을 래핑하고, 값이 없음을 나타낸다.

- 옵셔널 변수를 사용할 때, 값에 접근하려면 옵셔널 래핑을 수행해야 한다. 이것은 안전성을 유지하기 위한 방법입니다. 래핑을 해야 값을 가져올 수 있고, 값이 없는 경우 런타임 오류가 발생하지 않는다.
- 래핑 방법은 옵셔널 바인딩 (Optional Binding), 옵셔널 체이닝 (Optional Chaining), 강제 언래핑 (!), 기본값을 설정하는 방법 등이 있다.

아래는 코드로 나타낸 예시
```swift
var nonOptionalInt: Int = 42 // 정수 값 
var optionalInt: Int? = 42 // 옵셔널로 래핑된 정수 값 또는 nil
```


- Wrapped는 Optional에 의해 wrapping되는 자료형을 의미한다.
밑에 코드는  Wrapped된 형식
```swift
public enum Optional<Wrapped> : ExpressibleByNilLiteral { 
	// 접근제한자는 public 열거형 Optional를 사용
	case none // 값이 없는 경우 (nil)
	case some(Wrapped)
	public init(_ some: Wrapped)
	// 중략
}
```

사용 예시
```swift
var name1: Optional<String> = "kjsung" // 값이 있을 때
var name2: Optional<String> = Optional.some("kjsung")
var name3: Optional<String> = .some("kjsung")

var name4: Optional<String> = nil // 값이 없을 때
var name5: Optional<String> = Optional.none
var name6: Optional<String> = none
```

예시 2
```swift

func checkOptionalValue(optionalValue: String?) { // Optional로 표기
	switch optionalValue {
		case .some(let value): // 값이 있는 경우
			print("This optional variable is \(value)")
		case .none: // 값이 없는 경우 (nil)
			print("This optional variable is nil")
	}
}

var name: String? = "kjsung"
checkOptionalValue(optionalValue: name) 
// 실행 결과: This optional variable is kjsung
name = nil
checkOptionalValue(optionalValue: name) 
// 실행 결과: This optional variable is nil
```

예시 3
```swift
let numbers: [Int?] = [2, nil, -4, 100] // Optional이 들어간 정수형 리스트

for number in numbers {
	switch number {
		case .some(let value) where value < 0: // 값이 있는 경우
			print("Negative value: \(value)")
		case .some(let value) where value > 10:
			print("Large value: \(value)")
		case .some(let value):
			print("Value: \(value)")
		case .none: // 값이 없는 경우 (nil)
		print("Value: nil")
	}
}

/* 실행 결과
Value: 2
Value: nil
Negative value: -4
Large value: 100
*/
```

nil 사용의 예시
함수에 전달되는 인자의 값이 잘못된 경우, 이 오류를 알리기 위해 함수 내에서 nil을 반환한다.

```swift
enum School: String {
	case primary = "Primary School"
	case elementary = "Elementary School"
	case Middle = "Middle School"
}

let sch1: School = School(rawValue: "Primary School")
// School(rawValue: "Primary School")은 nil이 될 수 있으므로 에러 발생
let sch2: School = School(rawValue: "Graduate School")
// School(rawValue: "Graduate School")은 nil이 될 수 있으므로 에러 발생

// 옵셔널을 붙여서 에러를 방지할 수 있다.
let sch3: School? = School(rawValue: "Primary School") //.primary
let sch4: School? = School(rawValue: "Graduate School") // nil

let sch5 = School(rawValue: "Primary School") 
// primary (컴파일러는 School(rawValue: "Primary School")은 nil이 될 수 있으므로 sch5를 School? 타입으로 타입 추론)
let sch6 = School(rawValue: "Graduate School") // nil (컴파일러는 School(rawValue: "Graduate School")은 nil이 될 수 있으므로 sch6를 School? 타입으로 타입 추론)

switch sch5 {
	case .some(School.primary):
		print(School.primary.rawValue)
	case .some(School.elementary):
		print(School.elementary.rawValue)
	case .some(School.middle):
		print(School.middle.rawValue)
	case .none:
		print("nil")
}
// 실행결과: Primary School
```


#### Optional 형으로 wrapping되어 있는 자료형(즉, Wrapped)으로 선언된 변수의 값(optional value)이 some 케이스로 존재할 때(nil이 아닐 때) 추출하는 방법

• Forced unwrapping
• Optional binding
• nil coalescing operator


#### Forced unwrapping
- Optional value가 존재하는 경우, Optional 형으로 wrapping되어 있는 자료형으로 선
언된 변수 옆에 느낌표(!) 붙여서 Optional value를 강제 추출한다.
- Optional value가 nil인 경우, 강제 추출하면 에러 발생한다. (추출할 것이 없는데 강제로 추
출하여 에러 발생)
- 따라서, forced unwrapping 전에 반드시 nil check 해야 한다.
- forced unwrapping는 Optional unwrapping 방법 중에서 가장 간단하지만 가장 위험한 방법이므로 forced unwrapping 대신 Optional Binding을 사용한다.

```swift
var nameA: String? = "Lee"
var nameB: String = nameA! // forced unwrapping (강제 추출)

// 1. nil check한 경우
if nameA != nil {
	print("My name is \(nameA!)") 
	// forced unwrapping (강제 추출)전에 반드시 nil check 해야 함
} else {
	print("My name == nil")
}
// 실행 결과: My name is Lee

// 2. nil check하지 않은 경우
nameA = nil
nameB = nameA! // Optional value가 nil인 경우, 강제 추출하면 에러 발생
```

#### Optional binding 
• Optional value가 존재하면 일정 블록 안에서 Optional unwrapping된 형태로 상수나 변수로 바인딩한다.
• Optional value가 존재하지 않으면(nil 이면) 통과한다.
• if, while, guard 구문과 결합하여 사용한다.

```swift
var strA: String? = "Kim"
var strB: String? = "Lee"

// 1. Optional binding을 통한 임시 상수 할당
if let name = strA {
	print("My name is \(name)")
} else {
	print("My name == nil")
}
// 실행 결과: My name is Kim

// 2. Optional binding을 통한 임시 변수 할당
if var name = strA {
	name = "Park"
	print("My name is \(name)")
} else {
	print("My name == nil")
}
// 실행 결과: My name is Park

// 3. Optional binding을 통한 여러 임시 상수 할당
if let name = strA, let friend = strB { // ,는 and의 의미
	print("\(name) and \(friend) are friend")
} else {
	print("name and friend are nil")
}
// 실행 결과: Kim and Lee are friend
```

#### nil 병합 연산자(nil coalescing  operator)
- A ?? B
- A가 nil이 아니면 A의 Optional unwrapping된 형태를 반환
- A가 nil이면 B를 반환

```swift
// 1. nil coalescing operator를 이용하지 않은 경우
if someNumber != nil {
	number = someNumber!
} else {
	number = 10
}

print(number) // 실행 결과: 10

// 2. nil coalescing operator (??)를 이용한 경우
var someNumber: Int? = nil
var number: Int = 0

number = someNumber ?? 10 
// A ?? B: A가 nil이 아니면 A의 unwrapping된 형태를, nil이면 B를 반환
print(number)
```

#### Impicitly Unwrapped Optional
- Optional value가 존재할 때 Optional unwrapping이 필요 없는 Optional 형
- Optional 형으로 wrapping된 자료형으로 선언된 변수의 값은 있을 수도 없을 수(nil)도 있음
- 사용방법: 자료형 옆에 느낌표(!) 붙임

```swift
var myName: String! = "Kim" // String!: implicitly unwrapped optional

print(myName)
// 실행 결과: Optional("Kim")

print(myName + " and Park") // optional unwrapping 하지 않아도 됨
// 실행 결과: Kim and Park

myName = nil // optional 변수이므로 nil 사용 가능

// Optional binding 가능
if let name = myName {
	print("My name is \(name)")
} else {
	print("My name == nil")
}
// 실행 결과: My name == nil

var index1: Int?

index1 = 3

if index1 != nil {
	print(index1) // 실행 결과 Optional(3)
	print(index1! + 3) // 실행 결과 6
}

var index1: Int! // implicitly unwrapped optional

index1 = 3

if index1 != nil {
	print(index1) // 실행 결과 Optional(3)
	print(index1! + 3) // 실행 결과 6
}

```


```swift
class Person {
	var height: Float = 0.0
	var weight: Float = 0.0
	
	init() { // 기본 생성자 재정의
		self.height = 10.0
		self.weight = 20.0
	}
	deinit { // 소멸자 재정의
		print("Person class instance is deinitialized")
	}
}

var aPerson: Person? = Person()
print(aPerson) // 실행 결과: Optional(__lldb_expr_13.Person)

//aPerson.height = 1.0 // 에러 발생: aPerson를 unwrapping해야 height에 접근 가능
//print(aPerson.height) // 에러 발생: aPerson를 unwrapping해야 height에 접근 가능

if let bPerson = aPerson { // optional unwrapping
	bPerson.height = 0.0 // 이 코드가 없으면 원래 10.0 이나 reference이므로 0.0 대입
	print(bPerson.height) // 실행 결과: 0.0
}

var cPerson: Person! = Person() // implicitly unwrapped optional

cPerson.height = 1.0
print(cPerson.height) // 실행 결과: 1.0
```
