- [[데이터베이스(DataBase)]]에 [[테이블(Table)]]의 값([[행(Row)]])을 UPDATE할 때 사용하는 문법으로. SET과 같이 연결하여 사용한다.

- [[WHERE]] 조건이 없는 경우에는 [[테이블(Table)]]에 있는 필드([[열(Column)]]) 전체가 변경할값으로 SET 된다.
- 따라서 거의 [[WHERE]]와 함께 쓰인다.


## 문법

- UPDATE 테이블 SET 열 = '변경할값' WHERE 조건 형식으로 사용한다.
  
```sql
UPDATE 테이블명 SET 속성명 = 변경할 값 WHERE 조건;
```

## 예시

- [[SELECT]] * FROM userTbl를 통해 밑의 이미지와 같은 userTbl이 있다고 가정하자.

![](https://t1.daumcdn.net/cfile/tistory/2508683B51AB2D0805) 

- 위의 예시에서 지금은 완전 NULL밖에 없다.
- NULL 값을 SET 하는 경우는 조건을 넣어주면 됩니다. 

```sql
UPDATE [테이블] SET [열]= '변경할값' WHERE [열] IS NULL
```

- 반대로 NULL이 아닌 값을 찾아 업데이트 해주는 방법도 있다.
- NULL 부분은 NOT NULL로 변경 해주면 된다.

```sql
UPDATE [테이블] SET [열]= '변경할값' WHERE [열] IS NOT NULL
```

- 밑에 예시는 UPDATE를 통해 모든 유저에게 Money 10000원 과 아이템을 하나씩 지급해보도록 가정한다.
 
```sql
UPDATE userTbl SET Money = 10000, item1 = '티셔츠'
-- 조건이 없으니 userTbl의 Money 와 Item1 전체에 적용된다.

SELECT * FROM userTbl 
```

![](https://t1.daumcdn.net/cfile/tistory/2614764651AB2D091C) 

- 이제 조건을 걸어 핸드폰 번호를 등록하지 않은 사람에게 돈을 천원씩 뺏어 가도록 해본다.
- 그리고 코멘트에는 미등록 이라고 넣어준다.

```sql
UPDATE userTbl SET Money = Money - 1000 WHERE Phone IS NULL
-- userTbl 에 Phone 의 값이 NULL 일 경우 money -1000 을 한 값을 money에 넣어준다.

UPDATE userTbl SET Comment = '미등록' WHERE Phone IS NULL
-- userTbl 에 Phone 의 값이 NULL 일 경우 미등록 을 comment 에 넣어준다.

SELECT * FROM userTbl
```

![](https://t1.daumcdn.net/cfile/tistory/27588E3951AB2D0923) 

