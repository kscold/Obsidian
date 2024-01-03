정렬되어 있지 않은 배열 안의 어떤 값을 찾을 때 순서대로 하나씩 검사하는 알고리즘

예시는  정수 배열 `data`, 배열의 길이 `n`, 그리고 찾고자 하는 `target`을 입력받아서 `target`을 찾으면 해당 인덱스를 반환하고, 찾기 못하면 -1을 반환하는 함수이다.
```python
def search(data, n, target):
    for i in range(n):
        if data[i] == target:
            return i
    return -1
```


→ 이 함수의 미션은 data[0]부터 data[n-1] 사이에서 target을 검색한다. 하지만 검색 구간의 시작 인덱스 0은 보통 생략한다. 즉 0은 '암시적(implicit) 매개변수' 이다.


매개 변수의 명시화 : 순차 탐색  
→ 이 함수의 미션은 data[begin]부터 data[end]사이에서 target을 검색한다. 즉, 검색구간의 시작점을 명시적(explicit)으로 지정한다.

```python
def search(data, begin, end, target):
    if begin > end:
        return -1
    elif target == data[begin]:
        return begin
    else:
        return search(data, begin + 1, end, target)
```