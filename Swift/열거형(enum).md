

열거형 생성자
• 열거형 생성자에서 열거형의 [[인스턴스(instance)]]는 case중 하나의 값으로 초기화해야 한다.

```swift
enum Student {
	case elementary, middle, high
	case none
	
	init() {
		self = .none // 열거형의 인스턴스는 case중 하나의 값으로 초기화해야 함
	}
	
	init(koreanAge: Int) {
		switch koreanAge {
			case 8...13:
				self = .elementary // 열거형의 인스턴스는 case중 하나의 값으로 초기화해야 함
			case 14...16:
				self = .middle
			case 17...19:
				self = .high
			default:
				self = .none
		}
	}
		
	init(bornYear: Int, currentYear: Int) {
		self.init(koreanAge: currentYear - bornYear + 1) 
		// 생성자 내에서 다른 생성자 호출
	}
}

var student: Student = Student(koreanAge: 16)
print(student) // 실행결과: middle

student = Student(bornYear: 1998, currentYear: 2016)
print(student) // 실행결과: high
```