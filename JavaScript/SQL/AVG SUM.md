- [[집계 함수(Aggregate Function)]]로 AVG [[함수(Function)]]는 선택한 필드([[열(Column)]])의 평균을 계산하며, SUM은 선택한 필드([[열(Column)]]의 합을 계산한다.

- [[MIN MAX]]와 다르게 숫자인 값에 대해서만 연산이 가능하며, [[null]]값은 무시하고 계산한다.

- 따라서, NULL을 0으로 취급하여 평균을 구하고 싶다면 NULL을 0으로 지정하는 작업을 추가로 해야 한다.