- [[SQL]] 명령어로 INSERT를 사용해서 [[테이블(Table)]]에 [[행(Row)]]를 삽입한다.

## 문법

```sql
INSERT INTO 테이블명(속성명1, 속성명2, 속성명3, ...) VALUES (값1, 값2 값3, ...);
```

- 필드([[열(Column)]])명은 [[테이블(Table)]]의 인자 값은 실제 매개변수라고 생각하면 된다.


## 예시

```sql
INSERT INTO mens(name, age) VALUES ('Kidongg', '27'); -- mens이라는 테이블 name, age 칼럼에 'Kidongg', '27' 데이터 추가
```