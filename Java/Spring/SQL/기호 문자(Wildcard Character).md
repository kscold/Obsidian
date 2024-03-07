- 와일드 카드 문자는 문자열의 다른 문자를 대체하는 데 사용된다.

- 와일드 카드 문자는 SQL LIKE 연산자와 함께 사용된다.
- [[LIKE]] 연산자는 [[WHERE]] 절에서 열의 지정된 패턴을 검색하는 데 사용됩니다.

- [[LIKE]] 연산자와 함께 사용되는 두 개의 기호 문자(Wildcard Character)가 있다.

## % 

- 백분율 기호는 0, 1 또는 하나 이상의 character를 나타냅니다.
## _ 

- 밑줄은 하나의 character를 나타낸다.


- 즉, %는 ‘모든 문자’라는 의미고, _ 는 ‘한 글자’라는 의미이다.



## 예시

- 와일드 카드는 조합하여 사용할 수도 있다.

- 다음은 %와 `_`와일드 카드가있는 다른 [[LIKE]] 연산자를 보여주는 몇 가지 예시이다.

| **LIKE Operator** | **Description** |
| ---- | ---- |
| WHERE CustomerName LIKE ‘a%’ | “a”로 시작하는 모든 값 |
| WHERE CustomerName LIKE ‘%a’ | “a”로 끝나는 모든 값 |
| WHERE CustomerName LIKE ‘%or%’ | “or”이 있는 모든 값 |
| WHERE CustomerName LIKE ‘_r%’ | 두 번째 인덱스에 “r” 이 있는 모든 값 |
| WHERE CustomerName LIKE ‘a_%_%’ | “a” 로 시작하며 최소 3글자 이상인 모든 값 |
| WHERE ContactName LIKE ‘a%o’ | “a” 로 시작하여 “o” 로 끝나는 모든 값 |