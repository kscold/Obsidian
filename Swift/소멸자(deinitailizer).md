- [[인스턴스(instance)]]가 소멸될 때 필요한 과정 수행한다.
- [[클래스(class)]]에만 존재하고 클래스당 하나만 존재한다.
- 클래스는 소멸자를 기본적으로 제공하며 사용자가 재정의하여 사용 가능하다.
 - deInit 메소드로써 매개변수, 소괄호, 반환값이 없다.
 - 클래스의 [[인스턴스(instance)]]가 메모리에서 소멸되기 직전 호출된다.

```swift
class FileManager {
	var fileName: String
	init(fileName: String) { // 생성자(initializer)
		self.fileName = fileName
	}
	
	func openFile() {
		print("Open File")
	}
	
	func writeFile() {
		print("Write File")
	}
	
	func closeFile() {
		print("Close File")
	}
	
	deinit { // 소멸자(deinitializer)
		self.closeFile()
	}
}

var fileManager: FileManager? = FileManager(fileName: "abc.txt")

if let manager: FileManager = fileManager {
	manager.openFile() // 실행 결과: Open File
	manager.writeFile() // 실행 결과: Write File
}

fileManager = nil // 클래스의 인스턴스가 메모리에서 소멸됨(소멸자 호출됨)
// 실행 결과: Close File
```