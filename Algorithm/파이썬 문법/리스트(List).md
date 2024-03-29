- 원래 리스트란 값과 포인터를 묶은 노드라는 것을 포인터로 연결한 자료구조을 이야기하며, 인덱스가 없으므로 Head 포인터로부터 순서대로 접근해야 한다. 
- 즉, 접근하는 속도가 느리다.
- 리스트의 크기가 정해져있기 않기 때문에 크기를 변경하는 데이터를 다룰 때 적절하다.

- 위와 같은 특성을 가지고 있는데, 파이썬의 경우 배열과 리스트를 구분하지 않는다.
- 즉, A[] 형식으로 통일, 파이썬의 리스트는 배열의 특징도 모두 가지고 있는 상태로 구현되었다.

- 파이썬은 배열의 장점이 index로 접근가능하다는 점과 리스트의 장점인 가변적인 크기로 변한다는 장점을 가져왔기 때문에 알고리즘 문제에서 필수적으로 사용된다.

- 리스트는 a[start : end : step] 이런식으로 [[슬라이싱(slicing)]]을 할 수 있는데, step은 보폭이라고 하며 몇개씩 끊어서 가져올지와 방향을 정한다.

- 파이썬 list.sort()의 내장함수는 [[퀵 정렬(Quick Sort)]]를 기본으로 한다.