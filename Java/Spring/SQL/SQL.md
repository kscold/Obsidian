- SQL은 [[관계형 데이터베이스(Relational DataBase)]]를 관리하기 위해 설계된 프로그래밍 언어이다.
- MySQL, Oracle, PosrgreSQL, MariaDB 등에서 사용한다.

![[Pasted image 20240106023542.png]]

## SQL 문법

- 대소문자를 가리지 않는다.
- 세미콜론(;)으로 끝이난다.
- 고유 값은 따옴표(' ')로 감싸준다.
- 주석은 문장 앞에 -- 를 붙여서 사용힌다.

## SQL의 기본 명령

- SQL의 가장 기본 명령은 4가지가 있다.

### [[SELECT|SELECT]]

- [[레코드(Record)]] 조회
### [[INSERT]]

- [[레코드(Record)]] 추가
### [[UPDATE]]

- [[레코드(Record)]] 갱신
### [[DELETE]]

- [[레코드(Record)]] 삭제


## 추가적인 SQL 명령

### CREATE DATABASE

- 데이터 베이스 생성
### [[SHOW]]

- 데이터 베이스 보기
### [[USE]]

- 데이터 베이스 사용

### [[CREATE]]

-  테이블 생성

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