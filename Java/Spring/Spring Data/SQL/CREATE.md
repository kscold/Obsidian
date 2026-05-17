- [[테이블(Table)]] 및 [[데이터베이스(DataBase)]]을 생성할 때 사용한다.

## 문법

```sql
CREATE TABLE 테이블명 (
	속성명1 자료형,
	속성명2 자료형,
	속성명3 자료형,
	PRIMARY KEY (기본 키)
);
```

## 예시

```sql
CREATE TABLE emp_table(   
	emp_id      NUMBER           NOT NULL,
    emp_name    VARCHAR2(100)    NOT NULL,
    gender      VARCHAR2(10)         NULL,
    age         NUMBER               NULL,
    hire_date   DATE                 NULL,
    etc         VARCHAR2(300)        NULL,
    PRIMARY KEY (emp_id)                  
);
```


- 테이블을 생성할 때 테이블 명 앞에 [[스키마(Schema)]]를 지정해야 한다. 
- 스키마를 지정하지 않고 생성을 하면 기본 스키마 dbo에 테이블이 생성된다.(스키마.테이블명, dbo.emp)

- 데이터가 입력될 때 해당 칼럼에 NULL이 입력되지 않도록 하기 위해서는 NOT NULL을 저장해야 한다. 기본 값은 NULL이며 NULL은 생략해도 된다.

 ![[Pasted image 20231115000103.png]]

- 해당 칼럼에 기본 값을 지정하고 싶을 때는 칼럼 명 선언부 뒤에 기본 값을 지정하면 된다.
- DEFAULT 뒤에 괄호를 사용해도 되고 사용하지 않고 기본 값을 지정할 수도 있다.

```sql
empno    INT NOT NULL DEFAULT(1000) PRIMARY KEY
hiredate DATE DEFAULT GETDATE()
deptno   INT DEFAULT 10
```