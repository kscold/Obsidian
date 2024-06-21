- SQL은 [[관계형 데이터베이스(Relational DataBase)]]를 관리하기 위해 설계된 프로그래밍 언어이다.
- MySQL, Oracle, PosrgreSQL, MariaDB 등에서 사용한다.


## SQL 문법

- 대소문자를 가리지 않는다.
- 세미콜론(;)으로 끝이난다.
- 고유 값은 따옴표(' ')로 감싸준다.
- 주석은 문장 앞에 -- 를 붙여서 사용힌다.
- 일반적인 코딩과 다르게 = 는 대입의 의미가 아니라 수학과 같이 같다(`==`)는 의미이다.

## SQL의 기본 명령

- SQL의 가장 기본 명령은 4가지가 있다.

### [[SELECT|SELECT]]

- [[행(Row)]] 조회
### [[INSERT]]

- [[행(Row)]] 추가
### [[UPDATE]]

- [[행(Row)]] 갱신
### [[DELETE]]

- [[행(Row)]] 삭제


## 추가적인 SQL 명령

### CREATE DATABASE

- [[데이터베이스(DataBase)]] 생성
### [[SHOW]]

- [[데이터베이스(DataBase)]] 보기
### USE

- [[데이터베이스(DataBase)]] 사용

### [[CREATE]]

-  [[테이블(Table)]] 생성

```sql
CREATE TABLE 테이블명( 
	칼럼명 형식 
	공란 허용여부 
	자동증가여부 
); 

CREATE TABLE mens( 
	id INT AUTO_INCREMENT,
	name VARCHAR(32) NOT NULL, 
	age INT(12) DEFAULT '24',
	PRIMARY KEY(id) 
);
```

### [[DROP]]

- 테이블 삭제