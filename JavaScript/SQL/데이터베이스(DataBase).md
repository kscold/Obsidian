- [[관계형 데이터베이스(Relational DataBase)]]에서 하나의 데이터베이스 생성해 사용하는 방법이다.


## 데이터베이스 조회

- 데이터베이스 목록을 조회한다.

```sql
SHOW DATABASES;
```

## 데이터베이스 만들기

- test라는 이름의 데이터베이스를 만든다.

```sql
CREATE DATABASE test;
```

- 이미 존재하는 데이터베이스와 같은 이름의 데이터베이스를 만들려고 하면 에러가 난다. 
- 같은 이름의 데이터베이스가 존재하지 않을 때만 만들고 싶다면 IF NOT EXISTS를 추가한다.

```sql
CREATE DATABASE IF NOT EXISTS test;
```

- 마침표 등 특별한 문자를 포함하는 경우에 에러가 난다. 
- 백틱으로 이름을 감싸면 에러가 나지 않는다.

```sql
CREATE DATABASE `test.test`;
```

## 데이터베이스 삭제하기

- test 데이터베이스를 삭제한다.

```sql
DROP DATABASE test;
```


## 데이터베이스 이름 변경하기

- 데이터베이스 이름을 바로 변경할 수 없으므로, 다른 방법을 이용한다.

- 예를 들어 test를 test1로 이름을 바꾸고 싶다면 먼저 test1 데이터베이스를 만든다.
- 이후 mysqldump 명령어를 통해 test를 백업한다.

```bash
mysqldump -u root -p test > test.sql
```

- test1로 복원한다.

```bash
mysql -u root -p test1 < test.sql
```