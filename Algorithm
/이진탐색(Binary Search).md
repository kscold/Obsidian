
- 시간복잡도 : O(log N)
- 정렬된 자료를 반으로 나누어 탐색하는 방법
- 주의점 : 자료는 `오름차순` 으로 정렬된 자료여야 한다.
- 이진트리, 바이너리서치는 코딩 인터뷰 단골문제
- `퍼포먼스가 아주 좋고` 구현하는 중에 dynamic programming recursion을 볼 수 있다.

- linear search (순차검색) : 순서대로 찾는다. 성능평가시 비교대상으로 사용한다.
- linear search는 정렬 방식이 상관 없다.
- binary search (이진탐색) : `반드시 정렬된 상태`에서 시작해야한다. [로그실행시간](http://hyeonstorage.tistory.com/335)을 보장한다.
- 참고로 insert sort (삽입정렬), bubble sort, selection sort는 O(n^2)의 성능을 갖고 있다.

- target : 찾고자 하는 값
- data : 오름차순으로 정렬된 list
- start : data 의 처음 값 인덱스
- end : data 의 마지막 값 인덱스
- mid : start, end 의 중간 인덱스

- 자료의 중간 값이 (mid) 찾고자 하는 값인지 검사
- 아니라면 대소관계를 비교하여 start, end 값 이동
- 동일 연산 반복 (재귀로 구현 가능)