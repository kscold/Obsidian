
집합에 관련된 것을 처리 하기 위해 만들어진 자료형이다.

set 키워드를 사용하거나 중괄호를 이용해서 표현할 수 있다.

```python
s1 = set({1, 2, 3})  
s2 = set([1, 2, 3])  
s3 = {1, 2, 3}
```

위의 코드 세개 다 같은 집합을 만든다.

비어있는 집합을 만들기 위해서는 아래와 같이 사용한다.

s4 = set()

교집합

```python
s1 = set([1, 2, 3, 4, 5])
s2 = set([4, 5, 6, 7, 8])

# 교집합 메서드 intersection
print(s1.intersection(s2))

# 교집합 연산자 &
print(s1 & s2)
```

**교집합**을 구할때는 "intersection" 이라는 메서드를 이용하거나 "&"를 이용하여 집합의 교집합을 구할 수 있습니다.  

{4, 5} 가 출력됨


합집합

```python
s1 = set([1, 2, 3, 4, 5])
s2 = set([4, 5, 6, 7, 8])

# 교집합 메서드 union
print(s1.union(s2))

# 교집합 연산자 |
print(s1 | s2)
```

{1, 2, 3, 4, 5, 6, 7, 8} 이 출력됨

차집합

```python
s1 = set([1, 2, 3, 4, 5])
s2 = set([4, 5, 6, 7, 8])

# 교집합 메서드 difference
print(s1.difference(s2))
print(s1.difference(s2))

# 교집합 연산자 -
print(s1 - s2)
print(s2 - s1)
```

결과는 차례대로 
{1, 2, 3}
{8, 6, 7}
{1, 2, 3}
{8, 6, 7}