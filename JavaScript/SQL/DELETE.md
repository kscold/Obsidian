 - DELETE 다음에 데이터([[행(Row)]])를 삭제할 대상 [[테이블(Table)]] 명을 명시하고, 해당 [[테이블(Table)]]에서 어떤 데이터를 지울 것인지 [[WHERE]] 절에 명시하면 된다.
 
 - [[WHERE]] 절을 생략하면 테이블에 있는 모든 데이터를 삭제한다.
 - 즉, [[행(Row)]]를 삭제한다.

 - 테이블명 앞에 FROM 구문은 생략 가능하다.

- [[WHERE]] 절은 주로 [[SELECT]] 문에서 많이 사용되지만, DELETE와 [[UPDATE]] 문장에서도 빼놓을 수 없는 구문이다.
- WHERE 절에 명시하는 조건은 값이 참(TRUE), 즉 조건을 만족했을 때 실행된다.

## 문법

```sql
DELETE
[FROM] -- []: 생략 가능
	테이블명
WHERE
	조건;
```

## 예시 

- 예를 들어 emp03 [[테이블(Table)]]의 emp_id 값([[행(Row)]])이 4인 ‘신사임당’ 데이터를 삭제하려면 다음과 같이 DELETE 문을 작성한다.

```sql
DELETE FROM emp03 WHERE emp_id = 4;
```


```sql
delete from movie
where title = 'Forrest Gump' -- 영화 제목이 Forrest Gump인 데이터를 삭제

delete from movie
where genre = 'Drama' -- 영화 장르가 Dream인 데이터를 삭제
```