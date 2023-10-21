
- 자료형에 대한 별칭을 생성한다.
- C/C++ 언어에서의 형 정의(typedef)와 유사하다.

```swift
typealias MyInt = Int // Int형에 대한 alias인 myInt 생성
typealias MyDouble = Double // Double형에 대한 alias인 myDouble 생성
typealias MyString = String // String형에 대한 alias인 myString 생성
typealias TupIntDbl = (String, Int, Double) // (String, Int, Double)형에 대한 alias인 TupIntDbl 생성
typealias ArrStr = Array<String> // Array<String>형에 대한 alias인 ArrStr 생성
typealias DicStrInt = Dictionary<String, Int> // Dictionary<String, Int>형에 대한 alias인 DicStrInt 생성
typealias SetStr = Set<String> // Set<String>형에 대한 alias인 DicStrInt 생성

var int: MyInt = 3
var dbl: MyDouble = 3.1
var str: MyString = "joe"
var personG: PersonTuple = ("kang", 11, 160.23)
var arrayG: ArrStr = ["aa", "bb", "cc", "dd"]
var dictG: DicStrInt = ["aa": 1, "bb": 2, "cc": 3]
var setC: SetStr = ["aa", "bb", "cc"]
```

