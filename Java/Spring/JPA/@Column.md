- @Column 어노테이션은 [[객체(Object)]] [[Java/필드(Field)|필드(Field)]]와 DB [[테이블(Table)]] 컬럼([[Spring/SQL/필드(Field)|필드(Field)]])을 맵핑한다.

### @Column의 속성  

### 1. name 

- 맵핑할 테이블의 컬럼 이름을 지정  
### 2. insertable 

- [[엔티티(Entity)]] 저장시 선언된 필드도 같이 저장한다.
### 3. updateable 

- [[엔티티(Entity)]] 수정시 이 [[Spring/SQL/필드(Field)|필드(Field)]]를 함께 수정한다.
### 4. table 

- 지정한 [[Spring/SQL/필드(Field)|필드(Field)]]를 다른 테이블에 맵핑한다.
### 5. nullable 

- NULL을 허용할지, 허용하지 않을지 결정한다.
- 기본적으로 true이다.
### 6. unique

- 제약조건을 걸 때 사용한다.
### 7. columnDefinition

- DB 컬럼 정보를 직접적으로 지정할 때 사용한다.
### 8. length 

- varchar의 길이를 조정한다.
- 기본값으로 255가 입력된다.
### 9. precsion, scale 

- BigInteger, BigDecimal 타입에서 사용, 각각 소수점 포함 자리수, 소수의 자리수를 의미한다.