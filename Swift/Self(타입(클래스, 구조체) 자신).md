• 타입([[클래스(class)]], [[구조체(struct)]]) 자기 자신을 나타내는 [[프로퍼티(Property)]]이다.

```swift
struct SystemVolume {
	static var volume: Int = 5 // 구조체의 타입 프로퍼티
	
	static func mute() { // 구조체의 타입 메서드
		Self.volume = 0 // 구조체의 타입 프로퍼티 접근
	}
}

class Navigation {
	var volume: Int = 5
	
	func finishGuideWay() {
		SystemVolume.volume = self.volume // 구조체의 타입 프로퍼티 접근
	}
}

SystemVolume.volume = 10 // print(SystemVolume.volume) 실행결과 10
SystemVolume.mute() // print(SystemVolume.mute) 실행결과 0
```