- [[하위클래스(SubClass)]]는 [[부모클래스(Super Class)]]로부터 상속되는 [[인스턴스 메소드(instance method)]], 유형 메서드, 인스턴스 속성, 유형 속성 또는 구독자의 고유한 사용자 지정 구현을 제공할 수 있다. 이것을 오버라이딩이라고 한다.

밑에 코드는 오버라이딩을 보여주는 예시
```swift
class Name { //슈퍼 클래스
    var name = "Song"
    
    func myName() {
        print("my name is \(name)")
    }
}

class ourName : Name {  //하위 클래스
    var yourName = "Kim"
    
    override func myName() { // 슈퍼클래스(부모클래스)를 재정의 
        print("my name is \(name) and your name is \(yourName)")
    }
}

let song : ourName = ourName()
song.myName()
```

위에 코드에서 Name이라는 [[클래스(class)]]를 하나 만들고 이를 상속받는 ourName를 만들었고, Name 클래스에는 myName이라는 [[메소드(method)]]가 하나 있는 것을 확인할 수 있다. 따라서 ourName으로 돌아와서 override키워드를 이용해 myName을 재정의하여 song이라는 [[인스턴스(instance)]]를 하나 생성한 뒤 메소드를 호출할 수 있다.

