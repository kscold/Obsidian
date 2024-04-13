
- 시간복잡도 : O(log N)
- 이분 탐색이란, 오름차순으로 정렬된 배열을 반복적으로 반으로 나누어 target이 선택될 때까지 탐색하는 알고리즘이다.
- 정렬된 자료를 반으로 나누어 탐색하는 방법이다.
- 주의점 : 자료는 `오름차순` 으로 정렬된 자료여야 한다.
- `퍼포먼스가 아주 좋고` 구현하는 중에 [[DP(Dynamic Programming)]] recursion을 볼 수 있다.

![[Pasted image 20240105215609.png]]



- linear search (순차검색) : 순서대로 찾는다. 성능평가시 비교대상으로 사용한다.
- linear search는 정렬 방식이 상관 없다.
- binary search (이진탐색) : `반드시 정렬된 상태`에서 시작해야한다. 로그실행시간을 보장한다.
- 참고로 insert sort (삽입정렬), bubble sort, selection sort는 O(n^2)의 성능을 갖고 있다.

- target : 찾고자 하는 값
- data : 오름차순으로 정렬된 list
- start : data 의 처음 값 인덱스
- end : data 의 마지막 값 인덱스
- mid : start, end 의 중간 인덱스

- 자료의 중간 값이 (mid) 찾고자 하는 값인지 검사한다.
- 아니라면 대소관계를 비교하여 start, end 값 이동한다.
- 동일 연산 반복(재귀로 구현 가능)한다.


## 이진 탐색의 문제 특징
- 기본적인 이분탐색에서 요구하는 값은 항상 mid값중 최대이거나, 최소인 값들이라는 점이다.
- 그렇다는건 문제의 출력과 mid를 연결해서 생각하면 좀더 쉽게 생각할수 있다는 것이다.
- 나는 목표레벨 T가 결국 k를 여러명에게 나눠서 더해주었을 때, 가장 큰 mid값이라고 생각했다.

 
### 이진 탐색 구현
```python
def binary_search(target, data):
	data.sort() # 데이터를 먼저 정렬
    start = 0 # 시작 인덱스를 설정
    end = len(data) - 1 # 마지막 인덱스를 설정
    
    while start <= end: # 시작인덱스가 마지막 인덱스를 넘어서면 탐색을 다한 것이므로 종료
        mid = (start + end) // 2 # 중간 mid 인덱스 값을 설정
        
        if data[mid] == target: # mid 인덱스값이 target 값이면 탐색 종료
            return mid # 함수를 끝내버린다.
        elif data[mid] < target: # mid 인덱스값이 target 보다 작으면 start 값을 증가
            start = mid + 1
        else: # mid 인덱스값이 target 보다 크면 end 값을 감소
            end = mid - 1 
            
    return None # target 값을 찾지 못하면 None을 반환
```

- 이렇게 푸는 문제가 대부분이나 백준 1920 수 찾기의 경우 O(1000,000) 이라 힘들다.
- 따라서 O(n)인 리스트를 사용하지 않고 O(1)인 상수 시간인 set를 사용하여 풀면 된다.

- 이진 탐색은 아니지만 set과 in을 이용한 탐색
```java
import sys  
  
input = sys.stdin.readline  
  
n = int(input())  
data = set(map(int, input().split()))  
m = int(input())  
target = list(map(int, input().split()))  
  
for t in target:  
    if t in data:  
        print(1)  
    else:  
        print(0)
```