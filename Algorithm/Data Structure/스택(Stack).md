- 제일 처음 들어 간 데이터가 가장 마지막에 나오는 선입후출(First In Last Out) 구조 또는 후입선출(Last In First Out) 구조라고 한다.

```python
stack = []

stack.append(5)
stack.append(2)
stack.append(3)
stack.append(7)
stack.pop() # pop은 데이터를 꺼냄과 동시에 그 데이터를 return 해준다.
stack.append(1)
stack.append(4)
stack.pop()

print(stack) # 최하단 원소부터 출력
print(stack[::-1]) # 최상단 원소터 출력

>> [5, 2, 3, 1]
>> [1, 3, 2, 5]
```