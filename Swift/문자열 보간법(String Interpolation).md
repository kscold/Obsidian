
문자열 내에 변수 또는 상수 값을 넣고 싶을 때에는 문자열 내에 \(변수 또는 상수)로 표기한다.

```swift
var strVal: String = "SMU"
var intVal: Int = 2022
let fltVal: Float = 0.1
str5 = "Hello \(strVal) \(intVal) ver.\(fltVal)" // "Hello SMU 2022 ver.0.1"
```


여러 줄의 문자열을 표현하기 위해 큰따옴표 3개(""")를 이용한다.
- 문자열 내에서 실제로 줄 바꿈을 하지 않지만 에디터 상에서 줄 바꿈을 표현하기 위해 백
슬래시(\) 사용한다.

```swift

var str6: String = """
The White Rabbit put on his spectacles. "Where shall I begin,
please your Majesty?" he asked.
"""
print(str6)

/* 결과
The White Rabbit put on his spectacles. "Where shall I begin,
please your Majesty?" he asked.
*/

var str7: String = """
The White Rabbit put on his spectacles. \
"Where shall I begin,
please your Majesty?" \
he asked.
"""
print(str6)

/* 결과
The White Rabbit put on his spectacles. "Where shall I begin,
please your Majesty?" he asked.
*/
```