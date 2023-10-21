- 배열 내의 element은 순서대로 저장된다.
- 배열 내의 element는 element의 index를 통해 접근 가능하면 element의 index는 0
부터 시작된다.
• 자료형: Array<자료형>
1. 배열 선언 및 초기화

```swift
var arrayA: Array<String>
var arrayB: [String]

arrayA = ["aa", "bb", "cc", "dd"] // 초기화를 따로하는 방법
arrayB = ["aa", "bb", "cc", "bb"]

var arrayC: Array<String> = ["aa", "bb", "cc", "dd"]

var arrayD: [String] = ["aa", "bb", "cc", "dd"]

var arrayE = ["aa", "bb", "cc", "dd"] // 변수 arrayE는 컴파일러에 의해 Array<String>형으로 타입 추론됨
```

2. 여러 자료형의 element로 구성된 배열

```swift
var arrayAnyA: Array<Any> = [1, 2.3, "c", "string"] // 모든 자료형 타입을 넣을 수 있음

var arrayAnyB: [Any] = [1, 2.3, "c", "string"]

var arrayAnyC = [1, 2.3, "c" , "string"] // 에러 발생: 여러 자료형의 element로 구성된 배열을 생성하려면 반드시 Any형을 명시해야 함

```

3. 빈 배열 생성
```swift

var emptyArrayA: Array<String> = Array<String>()
var emptyArrayB: [String] = [String]()
var emptyArrayC: [String] = []
var emptyArrayC = [] // 에러 발생: []를 사용하여 빈 배열을 생성하려면 반드시 자료형을 명시해야 함
```

4. typealias 이용
```swift
typealias ArrStr = Array<String>
var arrayG: ArrStr = ["aa", "bb", "cc", "dd"]
var emptyArrayD: ArrStr = ArrStr()
```


```swift
// 5. 빈 배열인지 확인 및 배열 내의 element 수 확인
print(arrayA.isEmpty) // false, ["aa", "bb", "cc", "dd"]가 들어 있음
print(emptyArrayA.isEmpty) // true, Array<String>() 선언만 했음
print(arrayA.count) // 4
print(emptyArrayA.count) // 0
```

 6. element 접근 및 수정
```swift
arrayA[0] = "smu"
arrayA[0] = arrayA[0] + arrayA[1] // smu, bb를 합친 값을 arrayA[0]에 대입

print(arrayA[0]) // "smubb"
print(arrayA.first) // "smubb", array[0]과 같은 의미
print(arrayA.last) // "dd"
print(arrayB.firstIndex(of: "bb")) // 1, bb 값이 있는 인덱스를 반환함
print(arrayB.lastIndex(of: "bb")) // 3
print(arrayB) // ["aa", "bb", "cc", "bb"]
print(arrayB[1...3]) // ["bb", "cc", "bb"] (범위 연산자(…)를 통한 접근)
```

7. element 추가
```swift
arrayC.append("ee") // ["aa", "bb", "cc", "dd", "ee"]
arrayC.append(contentsOf: ["ff", "gg"]) // ["aa", "bb", "cc", "dd", "ee", "ff", "gg"]
arrayD.insert("hh", at: 2) // ["aa", "bb", "hh", "cc", "dd"]
arrayD.insert(contentsOf: ["hh", "gg"], at: 2) // ["aa", "bb", "hh", "gg", "hh", "cc", "dd"]
```

8. element 삭제
```swift
var removedElement: String

removedElement = arrayD.removeFirst() // "aa"
removedElement = arrayD.removeLast() // "dd"
removedElement = arrayD.remove(at: 0) // "bb"

print(arrayD) 
```