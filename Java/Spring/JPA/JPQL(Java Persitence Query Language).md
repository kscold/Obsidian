- [[엔티티(Entity)]] [[객체(Object)]]를 대상으로 하는 [[객체지향(Object Oriented Programming)]] [[쿼리(query)]]이다.

- [[SQL]]을 추상화한 [[객체지향(Object Oriented Programming)]] [[쿼리(query)]]이며, 작성된 JPQL은 [[SQL]]로 변환된다.

- 기존 [[JPA(Java Persistence API)]]의 [[메서드(Method)]] 호출만으로는 섬세한 [[쿼리(query)]] 작성이 어렵다는 문제를 해결하기 위해 JPQL이 나타나게 되었으며 [[SQL]]을 추상화했기 때문에 특정 [[데이터베이스(DataBase)]] [[SQL]]에 의존하지 않는다는 장점이 있다.

- [[SELECT]], FROM, [[WHERE]], [[GROUP]] BY, HAVING, [[JOIN]]을 지원한다.


## JPQL 기본 문법

- [[엔티티(Entity)]] [[클래스(Class)]] 이름, [[엔티티(Entity)]] [[Spring/SQL/필드(Field)|필드(Field)]]의 대소문자가 일치해야한다. 
- 예를 들어 유저의 나이를 조회하는데 나이 필드가 age면 Age가 아닌 age로 작성한다.

- JPQL 키워드는 대소문자 구분하지 않는다. (SELECT, select, From, from)

- [[엔티티(Entity)]] [[객체(Object)]]를 대상으로 하는 쿼리이므로 엔티티 이름을 사용한다. (테이블 이름할 수 없다.)
- 별칭은 필수이다. (여기서는 u),  (as는 생략가능하다.)

- SQL을 추상화해서 특정 [[데이터베이스(DataBase)]] [[SQL]]에 의존하지 않는다.
- JPQL은 결국 [[SQL]]로 변환된다.

### 집합과 정렬

- COUNT, SUM, AVG, MAX, MIN 과 같은 집합 함수를 사용하여 결과를 집계할 수 있다.

```java
select 
	COUNT(u), // 유저 수    
	SUM(u.age), // 나이 합    
	AVG(u.age), // 평균 나이    
	MAX(u.age), // 최대 나이    
	MIN(u.age) // 최소 나이
from User u
```

- [[GROUP]] BY, ORDER BY 를 사용하여 [[엔티티(Entity)]]의 [[Spring/SQL/필드(Field)|필드(Field)]]로 그룹화하고, HAVING 절을 사용해 그룹 조건을 적용할 수 있다.

```java
// 각 나라별 유저 수
SELECT u.country, COUNT(u) FROM User u GROUP BY u.country 

// 각 나라별 유저 수가 > 100
SELECT u.country, COUNT(u) FROM User u GROUP BY u.country HAVING COUNT(u) > 100
```

### 프로젝션 (SELECT 절에 조회할 대상을 지정)

- 프로젝션 대상으로는 [[엔티티(Entity)]], 임베디드 타입, 스칼라 타입(숫자, 문자 등 기본 데이터 타입)이 있다.

```java
SELECT m FROM Member m // 엔티티 프로젝션
SELECT m.address FROM Member m  // 임베디드 타입 프로젝션
SELECT m.username, m.age FROM Member m  // 스칼라(숫자, 문자, 날짜와 같은 기본 데이터 타입) 타입 프로젝션
select distinct m.username, m.age from Member.m // 중복값이 있을 경우 DISTINCT로 중복 제거 가능
```

## JPQL의 join

- JPQL에서 [[JOIN]]은 [[엔티티(Entity)]] 간의 관계로 수행되며 주로 일대일, [[일대다(OnetoMany)]], [[다대일(ManyToOne)]], [[다대다(ManyToMany)]] 관계를 조회할 때 사용한다.
- JPQL [[JOIN]]은 연관된 [[Spring/SQL/필드(Field)|필드(Field)]]를 기준으로 [[JOIN]]하며 [[외래 키(Foreign Key)]]를 통해 자동으로 맵핑된다.

### 내부 조인 (INNER JOIN)

- [[연관 관계(Relationships)]]가 맺어진 [[엔티티(Entity)]]들에 대한 inner Join을 말한다. 
- INNER 는 생략가능하다.

- 특정 [[엔티티(Entity)]] 간의 관계를 기반으로 일치하는 결과만 반환한다.

- 아래 [[쿼리(query)]]에서는 u.group을 기준으로 User와 Group [[엔티티(Entity)]]가 결합되며, 일치하는 결과만 반환된다.

```java
SELECT u, g FROM User u JOIN u.group g
```

- 내부 조인을 사용할 때는 반드시 연결할 엔티티와 연관 필드가 정의되어야 하며, 일치하는 결과를 찾지 못할 경우 결과에 포함되지 않는다.

### 외부 조인 (LEFT OUTER JOIN , RIGHT OUTER JOIN)

- 한쪽 [[엔티티(Entity)]]에 연결된 엔티티가 누락되어도 결과 반환을 보장한다.
- OUTER는 생략 가능하다.

### LEFT JOIN

- 왼쪽 엔티티는 모두 선택하고, 오른쪽 엔티티는 일치하는 것만 선택한다.
### RIGHT JOIN

- 오른쪽 엔티티는 모두 선택하고, 왼쪽 엔티티는 일치하는 것만 선택한다

- 예를 들어, 테이블 A에는 A, B, C가 있고 테이블 B에는 A, B, D가 존재한다고 가정했을 때 LEFT JOIN을 수행하는 경우 테이블 A가 기준이 되고, 조인 결과로 A, B, C를 출력한다.
- 반대로 RIGHT JOIN을 수행하는 경우, 테이블 B가 기준이 되고, 조인 결과로 A, B, D를 출력한다. 

- 아래 [[쿼리(query)]]에서 "o.product"를 기준으로 Order와 Product 엔티티가 외부 조인되며, 일치하지 않더라도 Order 엔티티는 결과에 포함된다.

```java
SELECT o, p FROM Order o LEFT JOIN o.product p
```

### 세타 조인 (Cross Join)

- 연관 관계없는 엔티티를 조인하는 방법이다.

- 두 엔티티 간의 모든 경우의 조합을 생성한 후, [[WHERE]] 절의 비교 조건에 따라 결과를 필터링한다.
- 세타 조인은 일반적으로 두 [[테이블(Table)]] 간의 [[연관 관계(Relationships)]]가 정의되지 않은 경우에 사용한다.

- 단, 내부(INNER) 조인만 가능 하다.

```java
SELECT u, p FROM User u, Product p WHERE u.id = p.sellerId
```

### ON 절

- JPQL의 ON 절은 조인 조건을 구체화하기 위해 사용된다. 
- [[JOIN]] 대상을 필터링한다.

- User 엔티티와 연결된 Address 엔티티를 조회하며 특정 도시(city)에 속한 주소만 필터링하는 경우, 다음과 같은 JPQL 쿼리를 사용할 수 있다.

```java
SELECT u, a FROM User u LEFT JOIN u.address a ON a.city = '서울'
```

- "u.address"를 기준으로, "a.city = '서울'" 이라는 조건으로 ON 절을 이용하여 원하는 도시에 속한 주소만 조회한 [[쿼리(query)]]이다.

- 세타 조인은 내부 조인 방식만 가능하지만, ON절은 [[엔티티(Entity)]] 간의 연관 관계가 없이도 외부 조인이 가능하다.
- 즉, ON절을 사용해 [[연관 관계(Relationships)]]가 없더라도 외부 조인을 할 수 있다.

### 서브 쿼리

- 서브 쿼리(Subquery)는 한 개의 [[쿼리(query)]] 안에서 다른 [[쿼리(query)]]를 포함하는 기능이다.
- 서브 쿼리는 주로 [[WHERE]] 절, HAVING 절, [[SELECT]] 절에서 사용되며, 부모 쿼리와 함께 중첩되어 작성된다.

- 서브 쿼리를 사용하면 복잡한 [[쿼리(query)]] 연산을 가능하게 하고, 결과에 대한 추가 조건을 적용할 수 있다.
- 아래의 쿼리는 서브쿼리를 활용하여 모든 사용자의 평균 나이를 계산하고, 부모 쿼리는 평균 나이보다 많은 사용자를 반환하는 쿼리이다.

```java
SELECT u FROM User u WHERE u.age > (SELECT AVG(u2.age) FROM User u2)
```

- 추가로 서브쿼리에서 WEHER 절에서 사용할 수 있는 **EXISTS, ALL, ANY, SOME, IN**이 있다.
- 이 것을 사용하여 **특정 조건에 부합하는 데이터를 필터**링할 수 있다.

#### EXISTS  

- 부모 쿼리의 해당 레코드가 서브 쿼리 결과에 존재하는지 여부를 검사한다. 
- 만약 결과가 하나 이상 존재하면 참이다.
- NOT EXISTS은 반대이다.

```sql
-- User 엔티티와 연결된 Address 엔티티를 조회하며, "서울"에 위치한 주소가 해당 사용자에 존재하는지 확인
SELECT u FROM User u WHERE EXISTS (SELECT a FROM u.addresses a WHERE a.city = '서울')
```

#### ALL

 - 부모 쿼리의 값이 서브쿼리의 모든 결과 값과 지정된 비교 연산자에 따라 비교되어 참인 경우에 해당하는 값을 반환한다.

```sql
-- 부모 쿼리(직원의 연봉)가 서브쿼리에서 반환된 모든 값(Manager의 연봉)보다 큰 경우를 필터링하며, 해당하는 직원만 반환
SELECT e FROM Employee e WHERE e.salary > ALL (SELECT m.salary FROM Manager m)
```

#### ANY, SOME

- ANY와 SOME는 같은 기능을 수행한다.
- 부모 쿼리의 값이 서브쿼리의 결과 값 중 지정된 비교 연산자에 따라 참인 경우에 해당하는 값을 반환한다.

```sql
-- 부모 쿼리(직원의 연봉)가 서브쿼리에서 반환된 값 중 하나(Manager의 연봉)보다 큰 경우를 필터링하며, 해당하는 직원을 반환
SELECT e FROM Employee e WHERE e.salary > ANY (SELECT m.salary FROM Manager m)
```

#### IN

- 서브쿼리의 결과 중 하나라도 같은 것이 있으면 참이다.
- NOT IN은 반대이다.

- 서브 쿼리의 결과 중 일치하는 값을 가진 데이터를 모두 선택한다.

```sql
-- User 엔티티와 연결된 Address 엔티티를 조회하며, "서울"에 위치한 주소에 사는 사용자와 같은 나이를 가진 사용자를 조회
SELECT u FROM User u WHERE u.age IN (SELECT u2.age FROM User u2 WHERE u2.city = '서울')
```


### 타입 표현

- 문자 : 'HELLO', 'She''s'
- 숫자 : 10L, 10D, 10F
- boolean : TRUE, FALSE
- ENUM : jpabook.MemberType.Admin (패키지명 포함)
- 엔티티 타입 : TYPE(m) = Member (상속 관계에서 사용)


#### 조건식 - CASE

- CASE 조건식은 다음과 같이 사용할 수 있다.
- 기본 [[SQL]] 문법이랑 별 차이가 없다.

```java
// 기본 CASE 식select    case when m.age <= 10 then '학생요금'        when m.age >= 60 then '경로요금'        else '일반요금'    endfrom Member m // 단순 CASE 식select    case t.name         when '팀A' then '인센티브110%'        when '팀B' then '인센티브120%'        else '인센티브105%'    endfrom Team t
```

> **이 외 조건식**

- COALESCE : 하나씩 조회해서 NULL이 아니면 반환
- NULLIF : 두 값이 같으면 NULL 반환, 다르면 첫번째 값 반환

```java
// 사용자 이름이 없으면 이름 없는 회원을 반환select coalesce(m.username,'이름 없는 회원') from Member m // 사용자 이름이 "관리자"면 NULL을 반환하고 나머지는 본인의 이름 반환select NULLIF(m.username, '관리자') from Member m
```

---

#### 기본 연산자

**다양한 연산자를 사용하여 조건을 지정**할 수 있다.

- 비교 연산자: =, <>, <, >, <=, >=
- 논리 연산자: AND, OR, NOT
- 멤버십 연산자: IN, NOT IN, ALL, ANY, SOME, EXISTS
- LIKE / BETWEEN ... AND: 패턴 매칭과 범위 검색을 위한 연산자

---

#### JPQL 기본 함수

- CONCAT() : 여러 문자열을 하나의 문자열로 혹은 여러 컬럼을 하나의 문자열로 합치는 함수
- SUBSTRING() : SUBSTRING(str,pos,len)의 형태로 사용하며 문자열을 자르는 함수
- TRIM() : 문자열에 공백을 제거하는 함수 ( 좌, 우 공백만 제거 )
- LOWER(), UPPER() : 문자를 소문자 대문자로 변경하는 함수
- LENGTH() : 문자열의 길이를 가져오는 함수
- LOCATE(substr, str) : str에 있는 문자열 substr의 검색 위치를 정수로 반환하는 함수 (substr이 str에 없으면 0 반환)
- ABS() : 절대값을 구하는 함수
- SQRT() : 제곱근을 반환하는 함수
- MOD(n, m) : n을 m으로 나눈 나머지를 반환하는 함수
- SIZE() : 컬렉션의 크기를 구하는 함수 (JPA에서 사용)
- INDEX() : LIST 타입 컬렉션의 위치값을 구하는 함수 (JPA에서 사용)


