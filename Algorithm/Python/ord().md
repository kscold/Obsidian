- 파이썬 내장 함수로 ord 함수를 이용하여 유니코드 문자를 반환한다.

- 예를 들어 ord('a') 를 하면 97이라는 유니코드 숫자를 반환해준다.


## 예시

- 따라서 밑의 예시처럼 문자와 숫자가 합쳐져 있는 알고리즘을 풀 때 전부 숫자 행렬 데이터로 만드는데 유용하게 쓰인다.

```python
input_data = input() # 예를 들어 a1를 입력
row = int(input_data[1])  # 문자열의 2번째는 숫자이므로 행을 추출
column = int(ord(input_data[0])) - int(ord('a')) + 1 # 문자열의 1번째 문자를 숫자로 만듬
```

- 열의 데이터를 추출하는 과정에서 ord 키워드를 사용하여 진행 입력 받은 문자를 처음 문자 a의 유니코드 수로 빼고 +1를 해줌으로써 문자 인덱스를 숫자 인덱스로 바꿀 수 있다.