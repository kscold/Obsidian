- replace(매개변수1, 매개변수 2) 형식으로 문자열 매개변수 1일때 매개변수2 문자열로 대체한다.

### 백준 1343 폴리모니오에서의 활용
```python
# 민식이는 다음과 같은 폴리오미노 2개를 무한개만큼 가지고 있다. AAAA와 BB# 이제 '.'와 'X'로 이루어진 보드판이 주어졌을 때, 민식이는 겹침없이 'X'를 모두 폴리오미노로 덮으려고 한다. 이때, '.'는 폴리오미노로 덮으면 안 된다.  
# 폴리오미노로 모두 덮은 보드판을 출력하는 프로그램을 작성하시오.

boards = input()  
  
boards = boards.replace("XXXX", "AAAA") # 문자열 boards의 XXXX를 AAAA로 대체 후 저장
boards = boards.replace("XX", "BB") # 문자열 boards의 XX를 BB로 대체 후 저장
  
if "X" in boards: # 그래도 처리되지 않은 X가 남아있으면
    print(-1) # 실패
else: # 성공했으면
    print(boards) # 대체된 문제열을 반환

```