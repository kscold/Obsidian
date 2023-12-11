
UPDATE [테이블] SET [열] = '변경할값' WHERE [조건]

조건이 없는 경우에는 테이블에 있는 열 전체가 변경할값으로 UPDATE 된다.

### 형식
  
```sql
UPDATE 테이블이름 
SET 컬럼1 = 값1, 컬럼2 = 값2, ... 
WHERE 조건;
```

![](https://t1.daumcdn.net/cfile/tistory/2508683B51AB2D0805) 

지금은 완전 NULL밖에 없습니다. 

NULL 값을 UPDATE 하는 경우는 조건을 넣어주면 됩니다. 

UPDATE [테이블] SET [열]= '변경할값' WHERE [열] is **null** 

반대로 널이 아닌 값을 찾아 업데이트 해주는 방법도 있습니다 

NULL 부분은 NOT NULL로 변경 해주면 됩니다. 

UPDATE [테이블] SET [열]= '변경할값' WHERE [열] is not null 

그럼 일단 한번 UPDATE 해봅시다. 

모든 유저에게 Money 10000원 과 아이템을 하나씩 지급해보도록 하겠습니다. 

![](https://t1.daumcdn.net/cfile/tistory/22322F3551AB2D0907) 

UPDATE userTbl SET Money = 10000 , item1 = '티셔츠' 

--조건이 없으니 userTbl의 Money 와 Item1 전체에 적용됩니다. 

SELECT * FROM userTbl 

![](https://t1.daumcdn.net/cfile/tistory/2614764651AB2D091C) 

그럼 조건을 넣어볼까요 

핸드폰 번호를 등록하지 않은 사람에게 돈을 천원씩 뺏어 가도록 해봅시다. 

그리고 코멘트에는 미등록 이라고 넣어줘봅시다. 

![](https://t1.daumcdn.net/cfile/tistory/0317CC3851AB2D0914) 

UPDATE userTbl SET Money = Money - 1000 WHERE Phone is null 

--userTbl 에 Phone 의 값이 null 일 경우 money -1000 을 한 값을 money에 넣어줍니다. 

UPDATE userTbl SET Comment = '미등록' WHERE Phone is null 

--userTbl 에 Phone 의 값이 null 일 경우 미등록 을 comment 에 넣어줍니다. 

SELECT * FROM userTbl 

![](https://t1.daumcdn.net/cfile/tistory/27588E3951AB2D0923) 
