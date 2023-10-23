
- 감시자 프로퍼티를 사용하면 [[프로퍼티(Property)]]의 값이 변경됨에 따라 적절한 액션을 취할 수 있다. 프로퍼티의 값이 새로 할당될 때마다 호출되고 변경되는 값이 현재의 값과 같더라도 호출된다.

-  [[저장 프로퍼티(Stored Properties)]], [[연산 프로퍼티(Computed Properties)]], [[타입 프로퍼티(Type Properties)]]는 프로퍼티 감시자(willSet 메소드와 didSet 메소드)로 구현 가능하다.
- 따라서 프로퍼티 감시자는 지연 저장 프로퍼티에는 사용할 수 없다.

- 프로퍼티 감시자에는 프로퍼티의 값이 변경되지 직전에 호출되는 `willSet메소드`와 프로퍼티의 값이 변경된 직후에 호출되는 `didSet메소드`가 있다. 

- 저장 프로퍼티가 초기화될 때 프로퍼티 감시자는 호출되지 않는다.
- 연산 프로퍼티는 클래스를 상속받았을 때만 연산 프로퍼티를 재정의하여 프로퍼티 감시자 구현이 가능하다.

#### willSet와 didSet 메소드의 매개변수

- willSet 
	- 저장 프로퍼티가 변경될 값
	- 매개변수가 생략되면 newValue라는 매개변수가 자동 지정됨

- didSet 메서드의 매개변수
	- 저장 프로퍼티가 변경되기 전의 값
	- 매개변수가 생략되면 oldValue라는 매개변수가 자동 지정됨
	

- willSet메서드에서 전달되는 전달인자는 프로퍼티가 `변경될 값`이고, didSet메서드에서 전달되는 프로퍼티의 값은 `변경되기 전의 값`이다. 
- willSet, didSet의 메소드는 매개변수가 하나씩 있는데 매개변수를 따로 지정하지 않으면 `willSet메서드에는 newValue`, `didSet메서드에는 oldValue`가 매개변수로 자동으로 저장된다.

|   |   |
|---|---|
|프로퍼티 감시자 |저장 프로퍼티 |연산 프로퍼티|
|willSet 메서드 저장 프로퍼티가 변경되기 직전 호출됨| set 메서드가 실행되기 전에 호출됨|
|didSet 메서드 저장 프로퍼티가 변경된 직후 호출됨 |set 메서드가 실행된 후에 호출됨|

밑에 코드는 저장 프로퍼티에 프로퍼티 감시자 구현한 예시
```swift
import Swift

class Account {
	var credit: Int = 0 { // 저장 프로퍼티
		willSet(newCredit) { // willSet 메서드: 저장 프로퍼티가 변경되기 직전 호출됨
			print("잔액이 \(credit)원에서 \(newCredit)원으로 변경될 예정입니다.")
		}
		didSet(oldCredit) { // didSet 메서드: 저장 프로퍼티가 변경된 직후 호출됨
			print("잔액이 \(oldCredit)원에서 \(credit)원으로 변경되었습니다.")
		}
	}
}

let myAccount: Account = Account()
// 잔액이 0원에서 10000원으로 변경될 예정입니다.
myAccount.credit = 10000
// 잔액이 0원에서 10000원으로 변경되었습니다.
```


밑에 코드는 저장 프로퍼티에 프로퍼티 감시자 구현 (willSet 메서드와 didSet 메서드의 매개변수가 없는 경우)
```swift
// 저장 프로퍼티에 프로퍼티 감시자 사용(willSet 메서드와 didSet 메서드의 매개변수가 없는 경우)
import Swift

class Account {
		var credit: Int = 0 { // 저장 프로퍼티
		willSet { // willSet 메서드: 저장 프로퍼티가 변경되기 직전 호출됨
			print("잔액이 \(credit)원에서 \(newValue)원으로 변경될 예정입니다.")
		}
		didSet { // didSet 메서드: 저장 프로퍼티가 변경된 직후 호출됨
			print("잔액이 \(oldValue)원에서 \(credit)원으로 변경되었습니다.")
		}
	}
}

let myAccount: Account = Account()
// 잔액이 0원에서 10000원으로 변경될 예정입니다.
myAccount.credit = 10000
// 잔액이 0원에서 10000원으로 변경되었습니다.
```


밑에 코드는 연산 프로퍼티에 프로퍼티 감시자 구현한 예시
```swift
import Swift

class Account {
	var credit: Int = 0 { // 저장 프로퍼티
	
	willSet { // willSet 메서드: 저장 프로퍼티가 변경되기 직전 호출됨
		print("잔액이 \(credit)원에서 \(newValue)원으로 변경될 예정입니다.")
	}
	
	didSet { // didSet 메서드: 저장 프로퍼티가 변경된 직후 호출됨
		print("잔액이 \(oldValue)원에서 \(credit)원으로 변경되었습니다.")
	}
}

var dollarValue: Double { // 연산 프로퍼티
		get {
			return Double(credit / 1000)
		}
		
		set {
			credit = Int(newValue * 1000)
			print("잔액을 \(newValue)달러로 변경 중입니다.")
		}
	}
}

class ForeignAccount: Account {
	override var dollarValue: Double { // 연산 프로퍼티를 재정의하여 프로퍼티 감시자 구현
		
		willSet { // willSet 메서드: set 메서드가 실행되기 전에 호출됨
			print("잔액이 \(self.dollarValue)달러에서 \(newValue)달러로 변경될 예정입니다.")
		}
		
		didSet { // didSet 메서드: set 메서드가 실행된 후에 호출됨
			print("잔액이 \(oldValue)달러에서 \(self.dollarValue)달러로 변경되었습니다.")
		}
	}
}

let myAccount: ForeignAccount = ForeignAccount()
// 잔액이 0원에서 1000원으로 변경될 예정입니다. (저장 프로퍼티의 willSet 메서드가 호출됨)
myAccount.credit = 1000
// 잔액이 0원에서 1000원으로 변경되었습니다. (저장 프로퍼티의 didSet 메서드가 호출됨)

// 잔액이 1.0달러에서 2.0달러로 변경될 예정입니다. (연산 프로퍼티의 willSet 메서드가 호출됨)
// 잔액이 1000원에서 2000원으로 변경될 예정입니다. (저장 프로퍼티의 willSet 메서드가 호출됨)
// 잔액이 1000원에서 2000원으로 변경되었습니다. (저장 프로퍼티의 didSet 메서드가 호출됨)
myAccount.dollarValue = 2 // 잔액을 2.0달러로 변경 중입니다. (연산 프로퍼티의 set 메서드가 호출됨)
// 잔액이 1.0달러에서 2.0달러로 변경되었습니다. (연산 프로퍼티의 didSet 메서드가 호출됨)

```