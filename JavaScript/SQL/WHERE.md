- 어떠한 [[테이블(Table)]]에서 특정 조건을 걸어 내가 원하는 데이터를 가져오는 명령어이다.


## 예시

```sql
WHERE payment_method = 'kakaopay' -- 필드명 payment_method가 'kakaopay' 라는 레코드(행)를 가지고 있으면 반환
```

- payment_method 필드([[열(Column)]])에 kakapay라는 값이 있으면 데이터를 가져온다.
- 여기서 주의할 점은 kakaopay는 필드명이 아니기 때문에 꼭 ''를 붙여서 사용해야 한다.

- AND 혹은 OR를 사용하여 조건문을 추가적으로 걸 수 있다.
- 일반적인 조건문도 사용이 가능하다.

### 포인트가 30000점 보다 많은 유저 추출하기

- 'point'가 30000보다 큰 데이터만 보려면 밑에 예시코드와 같이 조건을 걸 수 있다.

```sql
SELECT * FROM point_users -- point_users 테이블에서 모든 필드를 가져옴
WHERE 'point' >= 300000
```


### 성이 이  씨인 유저만 추출하기

- 원하는 [[테이블(Table)]](users)에서 조건(name = '이**')인 데이터를 보려면 아래와 같이 [[쿼리(Query)]]문을 작성하면 된다.

- [[SQL]]에서는 일반적인 코딩과 다르게 = 는 대입의 의미가 아니라 수학과 같이 같다는 의미이다.

```sql
SELECT * FROM Users  
WHERE name = '이**' -- name 필드가 '이**'이랑 같으면 반환
```


### 앱 개발 종합반이면서 결제수단이 카카오페이인 데이터 추출하기

- 원하는 테이블 (orders)에서 조건 (course_title = '앱개발 종합반')이고 (payment_method = 'kakaopay)인 데이터를 보려면 아래와 같다.

```sql
SELECT * FROM orders  
WHERE course_title = '앱개발 종합반' AND payment_method = 'kakaopay'
```

### 같지 않은 데이터를 찾아서 데이터 추출하기

- 아래는 결제수단이 'card'이 외에 데이터 추출하는 예시이다.

- '!='에서의 !(느낌표)는 부정(not)을 의미하는 같지 않음의 조건을 걸기 위해서 느낌표를 사용한다.

```sql
SELECT * FROM  orders  
WHERE payment_method != 'card'
```

### 7월 13일~19일 등록(범위) 데이터 추출하기

- 범위 안의 데이터를 사용하기 위해서는 between 키워드를 사용하여 아래 예시와 같이 사용하면 된다.

- between을 사용할 때 주의해야할 점은 따옴표 ('')를 사용해야 하며 마지막 날짜는 '미만'을 뜻한다.

```sql  
SELECT * FROM enrolleds  
WHERE created_at BETWEEN '2020-07-13' AND '2020-07-20'
```

### 웹개발 종합반 관련(포함) 데이터만 추출하기

- 원하는 [[테이블(Table)]]에서 조건인 데이터를 보려면 아래와 같이 IN 키워드를 사용하여 쿼리문을 작성한다.

- IN 키워드를 사용할 때에는 포함하고 싶은 내용을 괄호 () 안에 넣어 쿼리문을 작성해야한다.
  
```sql
SELECT * FROM orders  
WHERE course_title IN ('웹개발 종합반')
```

### 이메일이 a로 시작해서 gmail인 데이터 추출하기

- 패턴이 있는 데이터를 추출하려면 원하는 테이블에서 조건에 맞는 데이터를 보려면 아래와 같이 쿼리문을 작성한다.

- 추출하려는 데이터에 특정한 패턴이 있다면  [[LIKE]] + %([[기호 문자(Wildcard Character)]]) 문법을 활용하여, % 앞에 특정 문자 또는 숫자를 넣으면 어떤 문자 및 숫자로 시작하는 데이터를, % 뒤에 특정 문자 또는 숫자를 넣으면 어떤 문자 및 숫자로 끝나는 데이터를 추출할 수 있다.

```sql
SELECT * FROM orders  
WHERE email LIKE 'a%gmail.com'  
```
