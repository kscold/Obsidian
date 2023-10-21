
• 자료형을 명시하는 것을 권장(디버깅할 때 좋고 컴파일 시간을 줄일 수 있음)

• 자료형을 명시하지 않으면 컴파일할 때 컴파일러에 의해 선언에 사용된 값을 통해
변수 또는 상수의 자료형이 추론될 수 있음(컴파일 시간이 오래 걸릴 수 있음)
값에 따라 변수 또는 상수의 타입 추론이 불가능할 수 있음

```swift
// 1. 변수 선언 및 값 설정
var intVar1: Int = 3
var dblVar1: Double = 3.1
var strVar1: String = "my name is"
// 2. 변수의 타입 추론
var intVar2 = 3 // 변수는 컴파일러에 의해 Int형으로 타입 추론됨
var dblVar2 = 3.1 // 변수는 컴파일러에 의해 Double형으로 타입 추론됨
var strVar2 = "my name is" // 변수는 컴파일러에 의해 String형으로 타입 추론됨
```