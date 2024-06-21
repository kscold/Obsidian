- 2개 이상의 [[테이블(Table)]] 묶어서 하나의 결과 [[테이블(Table)]]을 만드는 것이다.
- [[SELECT]]문과 같이 사용한다.

## Inner join(통상 조인)

- 교집합, 조인 조건을 만족하는 행만 출력하는 조인이다.

```sql
SELECT [COLUMN 목록]
	FROM [테이블1]
    INNER JOIN [테이블2]
    	ON [조인 조건]
WHERE [검색 조건]
```

![](https://blog.kakaocdn.net/dn/0dSZe/btqXwcCrisQ/Tju9MYaN7o8BpGK6kWEPZK/img.jpg)

## LEFT JOIN 

- 조인기준 왼쪽에 있는것 전부 [[SELECT]]된다.
- 공통적인 부분과 LEFT에 있는 데이터만 합쳐진다.

![](https://blog.kakaocdn.net/dn/HbXSf/btqBjVcc5nt/1IlFXWNOBg7VExLDL4kcF1/img.png)


- 테이블 A가 아래와 같다고 가정한다.

| ID  | ENAME |
| --- | ----- |
| 1   | AAA   |
| 2   | BBB   |
| 3   | CCC   |

- 테이블 B가 아래와 같다고 가정한다.

| ID  | KNAME |
| --- | ----- |
| 1   | 가     |
| 2   | 나     |
| 4   | 라     |
| 5   | 마     |


```sql
SELECT A.ID, A.ENAME, A.KNAME
	FROM A LEFT 
		OUTER JOIN B
			ON A.ID = B.ID;
```

| ID    | ENAME   | KNAME    |
| ----- | ------- | -------- |
| 1     | AAA     | 가        |
| 2     | BBB     | 나        |
| **3** | **CCC** | **NULL** |

## **Outer join** 

- 조인 조건을 만족하지 않는 행까지 포함해 출력한다.

```sql
-- LEFT OUTER JOIN
SELECT [COLUMN 목록]
	FROM [LEFT 테이블]
    [LEFT] OUTER JOIN [RIGHT 테이블]
    	ON [조인 조건]
WHERE [검색 조건]

-- RIGHT OUTER JOIN
SELECT [COLUMN 목록]
	FROM [RIGHT 테이블]
    [RIGHT] OUTER JOIN [LEFT 테이블]
    	ON [조인 조건]
WHERE [검색 조건]
```

## Cross join(상호 조인)

- 한쪽 테이블의 모든 행과 다른 쪽 테이블의 모든 행을 조인하는 것이다.
- 상호 조인 결과 테이블의 행수는 두 테이블의 행수를 곱한 값
- cartesian product라고도 불린다.

```sql
-- table1과 table2 상호 조인
SELECT *
	FROM [table1]
    	CROSS JOIN [table2]
        
-- count(*) 함수로 상호 조인 결과의 개수만 보기
SELECT COUNT(*)
	FROM [table1]
    	CROSS JOIN [table2]
```

## Self join 자체 조인

- 자기 자신을 조인하는 것이다.

- 조직도 관련 테이블을 예시로 들 수 있다.

![](https://blog.kakaocdn.net/dn/nLVyc/btqXj7I1Lvk/Ykapoh5xj0RpaPjxMqqwUK/img.png)


```sql
SELECT A.emp AS '부하직원', B.emp AS '직속상관', B.empTEL AS '직속상관연락처'
	FROM empTBL A /* empTBL 테이블 이름을 A로 설정 */
    	INNER JOIN empTBL B
        	ON A.manager = B.emp
    WHERE A.emp = '우대리';
```

- 밑의 이미지는 위 sql 쿼리문 실행 결과이다.

![](https://blog.kakaocdn.net/dn/bkoKle/btqXu5DCmfb/3cWsvoJ7YA2TCb6EcC4ryk/img.png)

