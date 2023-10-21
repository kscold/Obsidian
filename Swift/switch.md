
C/C++ 언어는 입력값으로 정수값을 사용하지만 Swift는 입력값으로 정수뿐 아니라 다양한 타입의 값 사용 가능

Swift는 각 case 문에서 실행이 끝나면 자동적으로 break 문을 실행함
- 따라서 case 문의 body에서 break 문을 생략 가능함
- 다음 case 문의 비교값과 비교하지 않고 다음 case 문의 body를 실행하려면 fallthrough문을 사용해야 함
- case문의 body에는 반드시 실행문이 있어야함
- [[범위 연산자]]도 사용가능함
- default로 어떤 case에도 해당이 안될 때 설정 가능
- float 및 double 형도 명시 가능
- case에서 변수에 입력값을 받아서 사용할수도 있음
- where를 통하여 추가적인 조건을 적용할 수 있다.

switch 문의 형식
```swift
switch 입력값 {
	case 비교값1:
		실행문
	case 비교값2:
		실행문
	case 비교값3, 비교값4, 비교값5 where 조건: 
	// 비교값3, 비교값4, 비교값5이고 조건일 때
		실행문
	default:
		실행문
}
```

```swift
// 1. 입력값이 정수인 경우
let intVal: Int = 5 // 정수 상수

switch intVal {
	case 0:
		print("intVal is 0") // break문 생략 가능
	case 1,2,3:
		print("1 <= intVal <= 3")
	case 3...10:
		print("3 <= intVal <= 10")
	case 20...30, 31..<40: // 범위 연산자: ..., ..< 20부터 30까지 31부터 39까지
		print("20 <= intVal <= 30 or 31 <= intVal < 40")
	default:
		print("40 <= intVal")
		break
}
// 출력 결과: 3 <= intVal <= 10

// 2. 입력값이 실수인 경우
let dblVal: Double = 6.44
switch dblVal {
	case 0:
		print("dblVal is 0")
	case 10.2: // 에러 발생: case body에 실행문이 있어야 함
	case 1.2, 3.4, 5.6:
		print("dblVal is 1.2 or 3.4 or 5.6")
	case 1.3...5.5:
		print("1.3 <= dblVal <= 5.5")
	case 6.0..<10.3, 15.2..<16.3:
		print("6.0 <= dblVal < 10.3 or 15.2 <= dblVal < 16.3")
	default:
		print("dblVal is real number")
}
// 출력 결과: 6.0 <= dblVal < 10.3 or 15.2 <= dblVal < 16.3

// 3. 입력값이 문자열인 경우
let strVal: String = "Jang"
switch strVal {
	case "Lee":
		print("He is Lee")
	case "KIM", "Park":
		print("He is KIM or Park")
	case "Jang":
		print("He is Jang")
		fallthrough // 다음 case문의 비교값과 비교하지 않고 다음 case문의 body 실행
	case "Son":
		print("He is Korean")
	default:
	print("I don't know who you are")
}
/* 출력 결과
He is Jang
He is Korean
*/

// 4. 입력값이 튜플인 경우
let tplVal1: (name: String, age: Int) = ("Jang", 23)
switch tplVal1 {
	case ("Jang", _): // 와일드 카드 식별자(_) 사용, 정수의 어떤 값이 들어가도 상관 없음
		fallthrough // 다음 case문의 비교값과 비교하지 않고 다음 case문의 body 실행
	case ("Lee", 20):
		fallthrough // 다음 case문의 body 실행
	case (name: "Kim", age: 20):
		print("\(tplVal1.name) is \(tplVal1.age)")
	default:
		print("I don't know who you are")
}
// 출력 결과: Jang is 23

// 5. 입력값이 튜플인 경우
let tplVal2: (name: String, age: Int) = ("Jang", 23)
switch tplVal2 {
	case ("Jang", let val): // case body에서 입력값을 상수(val)로 받아 사용
		print("\(tplVal2.name) is \(val)")
	case (name: "Kim", age: 20):
		print("\(tplVal2.name) is \(tplVal2.age)")
	default:
		print("I don't know who you are")
}
// 출력 결과: Jang is 23

// 6. 입력값이 열거형인 경우
enum School {
	case elementary, middle, high, university, graduate
}
let enumVal: School = School.high
switch enumVal {
	case .elementary:
		print("He is an elementary school student.")
	case .middle, .high:
		print("He is a middle or high school student.")
	default:
		print("He is a university or graduate school student.")
}
// 출력 결과: He is a middle or high school student.
```


```swift
// 7. 입력값이 연관 값(associated value)을 가진 항목들로 구성된 열거형인 경우
enum Device {
	case iPhone(model: String, storage: Int) // named
	case iMac(size: Int, model: String, price: Int)
	case macBook(Int, String, Int) // unnamed
	case watch
}

var giftA: Device = Device.iPhone(model: "Y", storage: 256)
var giftA: Device = Device.iPhone("Y", 256) // 에러 발생, key값이 존재하므로 명시해줘야 함
var giftB: Device = .iMac(size: 12, model: "X", price: 100)
var giftC: Device = .macBook(12, "X", 200) // 애초에 key값이 없으면 상관 없음
var giftD: Device = .watch // 아무런 자료형이 주어지지 않음

switch giftA {
	case .iPhone(model: "X", storage: 256):
		print("iPhone X and 256GB")
	case .iPhone(model: "Y", _): // 와일드카드(_) 사용, 주어진 자료형에서는 어떤 값도 받음
		print("iPhone Y")
	case .iPhone: // 연관값이 있는 또는 없는 경우
		print("iPhone")
	default:
		print("default")
}
// 출력 결과: iPhone Y

switch giftA {
	case .iPhone(let mdl, let stg): 
	// case body 내에서 연관값(model, stroage)을 상수(mdl, stg)로 받아 사용할 때 -> 패킹
		print("iPhone \(mdl) and \(stg)GB")
	case let .iMac(sz, mdl, prc): 
	// case body 내에서 모든 연관값(size, model, price)을 상수(sz, mdl, prc)로 받아 사용할 때 -> 모든 값을 받을려면 형식 선언 부 앞에 선언하면 됨
		print("iMac \(sz), \(mdl), \(prc)")
	default:
		print("default")
}
// 출력 결과: iPhone Y and 256GB

switch giftB {
	case .iPhone(let mdl, let stg): 
	// case body에서 연관 값(model, stroage)을 상수(mdl, stg)로 받아 사용할 때
		print("iPhone \(mdl) and \(stg)GB")
	case let .iMac(sz, mdl, prc): 
	// case body에서 모든 연관 값 (size, model, price)을 상수 (sz, mdl, prc)로 받아 사용 할 때
		print("iMac \(sz), \(mdl), \(prc)")
	default:
		print("default")
}
// 출력 결과: iMac 12, X, 100
```