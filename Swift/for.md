
Python의 for-in 문과 유사한 형태를 가짐
다른 프로그래밍 언어의 반복문처럼 break문과 continue 문 사용 가능
[[범위 연산자]]와 같이 쓰임

형식
```swift
for 상수 in 변수 {
	실행문
}
```

 1. 범위의 숫자인 경우
```swift
for i in 0...2 { // i는 상수
	print("i: \(i)")
}
/* 실행 결과
i: 0
i: 1
i: 2
*/
for i in 0...2 { // i는 상수 (for-in문 내에서만 사용되는 지역 상수)
	i += 1 // 에러 발생
	print("i: \(i)")
}

var i: Int // 이렇게 golbal로 선언하여도 밑에서 i는 지역 상수로 판단함

for i in 0...2 { // i는 상수 (for-in문 내에서만 사용되는 지역 상수) i는 현재 let임
	i += 1 // 에러 발생
	print("i: \(i)")
}

for var i in 0...10 { // i는 변수 (for-in문 내에서만 사용되는 지역변수)
	i += 1 // 이렇게 하면 대입 가능 var 변수이기 때문
	
	if i < 5 {
		print("i: \(i)")
		continue
	} else if i == 5 {
		break
	}
}
/* 실행 결과
i: 1
i: 2
i: 3
i: 4
*/

// 와일드카드 식별자(_) 사용
var result: Int = 1

for _ in 0...2 { // 0에서 2까지 _로 받기 때문에 따로 지역상수로 저장하지 않음
	result *= 10
}

print("result: \(result)")

/* 실행 결과
result: 1000
*/
```

2. 문자열인 경우
```swift
var str: String = "abc"

for char in str { // 문자열 내의 각 item은 문자로 분해됨, 배열과 똑같이 작용
	print("\(char)")
}

/*
a
b
c
*/
```

3. [[배열]]인 경우
```swift
var arr: Array<Double> = [1.1, 2.2, 3.3]

for n in arr { // 배열의 값을 하나하나 순회
	print("\(n)")
}

/*
1.1
2.2
3.3
*/
```

4. [[딕셔너리]]인 경우
```swift
var dic: Dictionary<String, Int> = ["Lee": 20, "KIM": 21, "Park": 22]

for tpl in dic { // 딕셔너리 내의 각 item은 튜플로 분해됨
	print("\(tpl)")
}

// 튜플로 분해되였기 때문에 밑에와 같이 표현
/*
(key: "Park", value: 22)
(key: "KIM", value: 21)
(key: "Lee", value: 20)
*/

for (name, age) in dic { // 딕셔너리 내의 각 item은 튜플로 분해됨
	print("\(name): \(age)")
}

/*
Park: 22
Lee: 20
KIM: 21
*/
```


5. [[집합]]인 경우
```swift
var set: Set<Double> = [1.1, 2.2, 3.3]
for n in set {
print("\(n)")
}
/*
1.1
2.2
3.3
*/
```