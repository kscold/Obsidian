재귀함수란 자기 자신을 다시 호출하는 함수를 의미한다.

재귀함수의 동작 과정을 보여주는 이미지
![[Pasted image 20231009174254.png]]
- 컴파일러 형태로 내려가서 가장마지막에 종료조건을 만족하면 다시 타고 올라오는 스택의 구조를 가지고 있음


단순한 형태의 재귀 함수 예제
- '재귀 함수를 호출합니다.' 라는 문자열을 무한히 출력한다.
-  어느 정도 출력하다가 최대 재귀 깊이 초과 메세지가 출력된다.

```python
def recursive_function():
	print('재귀 함수를 호출합니다.')
	recursive_function() # 함수가 본인 함수를 또 호출

recursive_funtion() # 파이썬 함수를 main 부에서 실행 -> return이나 종료조건이 없으므로 무한으로 반복이 됨
```

위의 예제가 생각보다 중요하다. 위의 함수를 실행 시 오류가 나는데 그 이유는 최대 재귀 깊이가 초과되었기 때문이다.
파이썬에서는 기본적인 재귀를 호출하는 과정에서 깊이 제한이 있기 때문에 별다른 설정을 하지 않고 이렇게 함수를 재귀적으로 호출하게 된다면 오류 메세지가 나올 수 있다.

따라서 위의 오류를 체감함으로서 함수가 재귀적으로 호출이 되면 컴퓨터 시스템의 스택 프레임에 이러한 함수가 반복적으로 쌓여서 가장 마지막에 호출된 함수가 처리가 된 이후에 그 함수를 불렀던 함수까지 처리가 되는 방식
-> 따라서 이런 구조와 스택과 매우 유사함 -> 컴퓨터의 메모리는 한정된 공간을 가지고 있기 때문에 파이썬은 재귀 깊이 제한을 가지고 있음

재귀 함수를 문제 풀이에서 사용할 때는 재귀 함수의 종료 조건(base case)을 반드시 명시해야 한다.
종료 조건을 제대로 명시하지 않으면 함수가 무한히 호출될 수 있다.

## recursive method의 기본 구성  

- 적어도 하나의 base case, 즉 순환되지 않고 종료되는 case가 있어야 함.
- 모든 case는 결국 base case로 수렴해야 함.

### base case 
- 재귀 호출에서 빠져나가기 위한 경우  
```python
if(n < 1):
	return 0
```  
    
### recursive case 
- 자신을 호출하는 함수를 포함하는 경우  
```python
else:
	myself(n - 1)
```

종료 조건을 포함한 재귀 함수 예제
```python
def recursive_function(i):
	# 100번째 호출을 했을 때 종료되도록 종료 조건 명시
	if i == 100:
		return # 리턴 조건이 return 이므로 스택이 pop될 시점에 종료 프린트를 호출
	print(i, '번째 재귀함수에서', i + 1, '번째 재귀함수를 호출합니다.')
	recursive_function(i + 1)
	print(i, '번째 재귀함수를 종료합니다.')

recursive_function(1)
```

위의 예시를 확인함으로서 재귀함수는 전부 호출 후 그 함수를 불렀던 함수까지 처리되는 방식이다.

팩토리얼 구현 예제
- n! = 1 x 2 x 3 x ... x (n-1) x n
- 수학적으로 0!과 1!의 값은 1입니다.

```python
# 반복적으로 구현한 n!
def factorial_iterative(n):
	result = 1
	# 1부터 n까지의 수를 차례대로 곱하기
	for i in range(1, n + 1):
		result *= i

# 재귀적으로 구현한 n!
def factorial_recursive(n):
	if n <= 1: # n이 1 이하인 경우 1을 반환
		return 1
	# n! = n * (n - 1)!를 그대로 코드로 작성하기
	return n * factorial_recursive(n - 1)

# 각각의 방식으로 구현한 n! 출력(n = 5)
print('반복적으로 구현', factorial_iterative(5))
print('재귀적으로 구현', factorial_recursive(5))
```

- 이렇게 위의 예시를 보면 알 수 있듯이 반복적인 구현 for 나 while의 경우 증가하는 조건을 거는 것에 반해 재귀적인 구현은 주로 감소하는 종료를 시킬 수 있는 조건을 거는데 초첨을 맞추고 있다.
- 따라서 재귀 함수를 사용했을 때 얻는 이점으로 수학에서 점화식을 그래도 소스코드에 옮길 수 있다는 장점이 있다.

수학적으로 생각해 보면 다음과 같다.
1. n이 0 혹은 1일 때: factorial(n) = 1
2. n이 1보다 클 때: factorial(n) = n X factorial(n-1)

- 일반적으로 우리는 점화식에서 종료 조건인 1번  n이 0 혹은 1일 때를 떠올릴 수 있다.

## 순환(Recursion)의 개념과 기본 예제

recursive method로 구성

문자열의 길이를 반환하는 예
```python
def length(s): # 문자열의 길이를 반환하는 코드
    if s == "": # 문자열이 비었으면 0을 반환
        return 0
    else:
        return 1 + length(s[1:]) # 문자열이 비었지 않았으면 두번째 문자부터 끝까지, 카운트를 1올림
```

문자열을 프린트 하는 예
```python
def printChars(s):
    if len(s) == 0:
        return
    else: # 문자열의 길이가 0이 아니면
        print(s[0])
        printChars(s[1:]) # [1:]로 슬라이싱 했기 때문에 종료조건까지 앞으로 1칸씩 감
```

문자열을 역순으로 한줄에 출력하는 재귀함수
```python
def printCharsReverse(s):
    if len(s) == 0:
        return
    else:
        printCharsReverse(s[1:]) # 먼저 1칸씩 앞으로 가서 종료조건까지 만족함
        print(s[0]) # 일전의 값들을 프린트함
```

최대공약수 계산(유클리드 호제법)의 예
- 두 자연수 A, B에 대하여 (A > B) A를 B로 나눈 나머지를 R이라고 한다.
- 이때 A와 B의 최대공약수는 B와 R의 최대공약수와 같다.
```python
def gcd(a, b):
	if a % b == 0: # a가 b보다 큰수이므로 b로 나눴을 때 나머지가 0이면 최대공약수는 b
		return b 
```

[[순차탐색(sequential search)]]의 다른 버전  
`data`는 배열, `begin, end`는 시작 끝 인덱스, `target`은 찾는 인덱스, 인덱스를 찾으면 찾은 인덱스를 반환, [[매개변수#명시적(explicit)]]
→ 중간에서 부터 앞 부분을 먼저 검색하고 그 안에서 target을 못 찾으면 뒷부분을 검색한다.

```python
def search(data, begin, end, target):
    if begin > end: # 시작 인덱스가 끝 인덱스보다 크면 -1 반환
        return -1
    
    else:
        middle = (begin + end) // 2 # 중간 인덱스 계산
        
        if data[middle] == target: # 배열의 중간 인덱스가 target이면 
            return middle # 중간 인덱스를 반환
            
        index = search(data, begin, middle - 1, target) # 왼쪽부터 재귀적으로 찾음
        
        if index != -1: # index가 -1이 아니면 index를 반환
            return index
        
        else:
            return search(data, middle + 1, end, target) # 오른쪽부처 재귀적으로 찾음
```


최대값 구하기  
→ 이 함수의 미션은 `data[begin]`과 `data[end]`사이에서 최대값을 찾아 반환하는 것이다. `begin ≤ end`라고 가정한다. [[매개변수#명시적(explicit)]]

```python
def findMax(data, begin, end)
	if begin == end:
		return data[begin]
	else:
		return max(data[begin], find_max(data, begin + 1, end))
```