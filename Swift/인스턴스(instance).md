
- 구조체나 클래스에서 인자가 들어가서 메모리 힙에 할당된 공간


값 타입(Call by value)의 [[구조체(struct)]] 인스턴스
```swift
struct BasicInformation { // 구조체 정의
	let name: String
	var age: Int
}

var kimInfo: BasicInformation = BasicInformation(name: "Kim", age: 99) 
// 인스턴스 정의
kimInfo.age = 100 // kimInfo는 다른 힙 메모리에 공간

var friendInfo: BasicInformation = kimInfo // 값 복사
print("kim age: \(kimInfo.age)")
print("friend age: \(friendInfo.age)")

/* 실행 결과
kim age: 100
friend age: 100
*/

friendInfo.age = 999 // call by value이므로 독자적인 공간으로 되어 있음
print("kim age: \(kimInfo.age)")
print("friend age: \(friendInfo.age)")
/* 실행 결과
kim age: 100
friend age: 999
*/
```



참조 타입(Call by reference)의 [[클래스(class)]] 인스턴스
```swift
class Person {
	var height: Float = 0.0
	var weight: Float = 0.0
}

var kim: Person = Person()
var friend: Person = kim // 참조 복사
var lee: Person = Person()

print("kim height: \(kim.height)")
print("friend height: \(friend.height)")
print("ref comparison (friend === kim): \(friend === kim)") // 참조 비교
print("ref comparison (friend === lee): \(friend === lee)") // 참조 비교
print("ref comparison (friend !== lee): \(friend !== lee)") // 참조 비교

/* 실행 결과
kim height: 0.0
friend height: 0.0
ref comparison (friend === kim): true
ref comparison (friend === lee): false
ref comparison (friend !== lee): true
*/

friend.height = 185.5
print("kim height: \(kim.height)")
print("friend height: \(friend.height)")

/* 실행 결과
kim height: 185.5
friend height: 185.5
*/
```


구초체 인스턴스와 클래스 인스턴스의 차이
```swift
func changeBasicInfo(_ info: BasicInformation) {
	var copiedInfo: BasicInformation = info
	copiedInfo.age = 1
}

func changeBasicInfo(_ info: Person) { 
// 함수 changeBasicInfo의 다중정의(overloading)
	info.height = 155.3
}

changeBasicInfo(kimInfo) // call by value
print("kim age: \(kimInfo.age)")

/* 실행 결과
kim age: 100
*/

changeBasicInfo(kim) // call by reference
print("kim height: \(kim.height)")

/* 실행 결과
kim height: 155.3
*/
```