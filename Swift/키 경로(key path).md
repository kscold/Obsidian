 
-  타입([[클래스(class)]], [[구조체(struct)]]) 내의 [[프로퍼티(Property)]]의 주소 경로(키 경로)이다.
-  형식은 `/타입.프로퍼티.프로퍼티.프로퍼티`로 사용한다.

 키 경로의 자료형
- WritableKeyPath<Root, Value>
	- 구조체 (값 타입) 내의 읽고 쓸 수 있는 프로퍼티의 주소 경로(키 경로)의 자료형
- ReferenceWritableKeyPath<Root, Value>
	- 클래스 (참조 타입) 내의 읽고 쓸 수 있는 프로퍼티의 주소 경로(키 경로)의 자료형

```swift
class Person {
	var name: String
	
	init(name: String) {
		self.name = name
	}
}

struct Stuff {
	var name: String
	var owner: Person
}

print(type(of: \Person.name)) 
// 실행 결과: ReferenceWritableKeyPath<Person, String>
print(type(of: \Stuff.name)) 
// 실행 결과: WritableKeyPath<Stuff, String>

let ownerKeyPath = \Stuff.owner
let nameKeyPath1 = \Stuff.owner.name
let nameKeyPath2 = ownerKeyPath.appending(path: \.name) 
// nameKeyPath1과 nameKeyPath2는 같음(같은 주소를 표시하는 다른 방법)
```


• 서브스크립트: [[인스턴스(instance)]] 뒤에 대괄호`[keypath: 경로]`를 사용하여 인스턴스 내의 프로퍼티에 접근 가능하도록 한다.

밑에 코드는 서브스크립트에 키 경로를 사용하여 프로퍼티에 접근하는 예시
```swift
class Person {
	var name: String
	
	init(name: String) {
		self.name = name
	}
}

struct Stuff {
	var name: String
	var owner: Person
}

let kim = Person(name: "kim")
let lee = Person(name: "lee")

let macbook = Stuff(name: "MacBook Pro", owner: kim)
var iMac = Stuff(name: "iMac", owner: lee)

let stuffNameKeyPath = \Stuff.name
let stuffOwnerKeyPath = \Stuff.owner
let stuffOwnerNameKeyPath = \Stuff.owner.name

print(macbook[keyPath: stuffNameKeyPath]) // 실행 결과: MacBook Pro
print(iMac[keyPath: stuffNameKeyPath]) // 실행 결과: iMac

print(macbook[keyPath: stuffOwnerNameKeyPath]) // 실행 결과: kim
print(iMac[keyPath: stuffOwnerNameKeyPath]) // 실행 결과: lee

//macbook[keyPath: stuffOwnerKeyPath] = kim // 에러 발생(상수인 macbook의 프로퍼티 변경 불가)
macbook[keyPath: stuffOwnerNameKeyPath] = "lee"
iMac[keyPath: stuffNameKeyPath] = "iMac Pro" // var이므로 원하는 구조체 위치에 값 변경 
iMac[keyPath: stuffOwnerKeyPath] = kim
```