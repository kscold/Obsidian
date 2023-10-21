- [[프로퍼티(Property)]] 명들을 매개변수로 하는 생성자이다.

```swift
struct Info {
	// struct 들의 property
	var name: String
	let age: Int
	
	init (name: String, age: Int){ // memberwise 생성자
	// 위에서 정의한 프로퍼티명들을 매겨변수로 만듬
		self.name = name
		self.age = age
	}
}
```