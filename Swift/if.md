
 Swift에서 if문 조건식의 값은 반드시 bool형 값(true와 false) 사용해야 함 

- 파이썬의 if문은 조건식의 값이 0이 아니면 true로, 0이면 false로 취급하지만 스위프트는 불가능함
- 즉 파이썬의 if 변수: // 변수가 존재하면

위의 같은 형식이 불가능하다는 뜻임
```Swift
let val1: Int = 3
let val1: Int = 4

if val1 > val2 { // 조건이 true false로 나옴
	result = val1
}

else if val1 < val2 {
	result = val1
}

else if val1 == val2 {
	result = 0
}

else {
	retusl = -1
}
```