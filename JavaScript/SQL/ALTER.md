- 기존 [[테이블(Table)]]에 새로운 필드([[열(Column)]])을 추가하거나 변경할때 쓰는 명령어이다.

- ALTER 명령은 [[CREATE]] TABLE문으로 만든 필드([[열(Column)]])은 삭제 할 수 없다.


## 문법

### 필드 추가

```sql
ALTER TABLE [TABLE NAME] ADD ([COLUMN NAME] DATATYPE);
```
### 필드명 수정

```sql
ALTER TABLE [TABLE NAME] RENAME COLUMN [COLUMN NAME] TO [NEW COLUMN NAME];
```
### 필드 타입 수정

```sql
ALTER TABLE [TABLE NAME] MODIFY ([COLUMN NAME] DATATYPE);
```
### 필드 타입 삭제

```sql
ALTER TABLE [TABLE NAME] DROP COLUMN [COLUMN NAME] );
```


## 예시
### 사용자 암호 변경

```sql
ALTER USER [USER ID] IDENTIFIED BY [NEW PASSWORD];

ALTER USER scott IDENTIFIED BY lion;
```
### 인덱스 수정

```sql

ALTER INDEX [INDEX NAME] RENAME TO [NEW INDEX NAME];

ALTER INDEX idx_empno RENAME TO idx_emp_01;  인덱스 idx_empno의 이름을 idx_emp_01로 변경함.
```

