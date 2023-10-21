
while 문의 조건식을 소괄호를 묶는 것은 생략 가능 파이썬과 비슷함
C언어의 do-while 문은 repeat-while문으로 구현 가능
다른 프로그래밍 언어의 반복문처럼 break문과 continue문 사용 가능

```swift
while 조건식 {
	실행문
}

repeat {
	실행문
} while 조건식
```

1. while문
```swift
var arr: [String] = ["Lee", "KIM"]
while arr.isEmpty == false { // 일반적인 조건 판단의 while문
print("name: \(arr.removeFirst())")
}
/* 실행 결과
name: Lee
name: KIM
*/
```

2. repeat-while문
```swift
arr = ["Lee", "KIM"]

repeat { // 먼저 실행하고 이후 조건을 만족하면 끝남
	print("name: \(arr.removeFirst())")
} while arr.isEmpty == false
/* 실행 결과
name: Lee
name: KIM
*/
```


스위프트는  중첩된 반복문을 구분하기 반복문에 이름 지정 가능

```swift
반복문명: for 상수 in 변수 {
	실행문
}
반복문명: while 조건식 {
	실행문
}
```

```swift
var numbers: [Int] = [3, 234, 6, 3252]

numbersLoop: for num in numbers { // while 문에 별명을 붙이고, 리스트를 순회
	if num > 5 || num < 1 { // 요소 조건 판단
		continue numbersLoop // while의 별명을 명시하여 continue
	}
	
	var count: Int = 0
	
	printLoop: while true{
		print(num) // 위에 조건으로 234, 6, 3252는 cotinue 되어 리스트 요소에서 빠짐
		count += 1 
		if count == num { // 3 프린트를 3번 반복
			break printLoop
		}
	}
	
	removeLoop: while true {
		if numbers.first != num {
			print("numbers: \(numbers)")
			break numbersLoop // 별명을 사용했기 때문에 정확히 어떤 반복문을 break할지 지정
		}
		numbers.removeFirst() // 리스트의 첫번째 요소를 pop
	}
}

/* 실행 결과
3
3
3
numbers: [234, 6, 3252] 
*/
```