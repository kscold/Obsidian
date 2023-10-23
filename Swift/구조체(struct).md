-  구조체의 인스턴스 생성 및 초기화 시 사용자 정의 [[생성자(initailizer)]] 가 없다면 기본적으로 자동 생성되는 [[memberwise]] 생성자를 사용한다.
- 구조체의 인스턴스 생성 및 초기화된 후 프로퍼티에 접근할 때에는 comma(.)를 사용한다.
- 구조체는 상속이 불가능하다.
- 구조체의 인스턴스는 값(value) 타입이다. (call by value)


#### 구조체의 인스턴스는 값타입
- 구조체의 인스턴스를 상수 let으로 선언하면 인스턴스 내부의 [[프로퍼티(Property)]] 변경 불가
- 구조체의 인스턴스를 변수 var으로 선언하면 인스턴스 내부의 [[프로퍼티(Property)]] 변경 가능
- 클래스 처럼 소멸자(deinitailizer)가 필요하지 않다.

```swift
struct Info { // 구조체 명 선언
	// 프로퍼티와 매소드들을 선언
	var name:String
	let age: Int
}

var alnfo: Info = Info(name: "Kim", age: 23) 
// 자동생성되는 memberwise 생성자를 통한 인스턴스 초기화
alnfo.name = "Park"
alnfo.age = 25 // let으로 선언되었으므로 인스턴스 내부의 프로퍼티 변경 불가 (에러)

var blnfo: Info = Info(name: "Lee", age: 24) // 인스턴스를 let으로 선언
blnfo.name = "Song" // 인스턴스 자체가 let으로 선언이 되었으므로 인스턴스 내부의 프로퍼티 변경 불가
blnfo.age = 26 // 마찬가지로 에러
```