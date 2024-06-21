- [[데이터베이스(DataBase)]]를 설계할 때, 하나의 [[테이블(Table)]] 만으로는 해당 테이블이 대표하는 데이터를 하나로 정리하기 힘들 수 있다.
- 이런 경우에는 [[테이블(Table)]]을 더 생성한 후, 외래 키를 사용하여 [[데이터베이스(DataBase)]] 내의 두 [[테이블(Table)]]을 이어주면 된다.


## 예시

- 예를 들어, 나라 정보가 쌓이는 country 테이블이 어떤 [[데이터베이스(DataBase)]] 안에 존재한다고 가정한다.

- 이 데이터베이스에 세계 대도시에 관한 정보도 쌓고 싶다.

- 제일 처음으로는 city [[테이블(Table)]]을 만든다.
- city [[테이블(Table)]]에는 도시 이름, 도시에게 부여되는 고유 숫자 코드, 도시의 국가 정보가 저장하고자 한다. 

- 추가적으로 나라 정보가 저장된 country [[테이블(Table)]]이 있다.
- country [[테이블(Table)]] 내에서 각 나라마다 부여된 고유 아이디를 city [[테이블(Table)]]에도 그대로 가져와 사용할 것이다.

- 이래야 두 [[테이블(Table)]]이 서로 연관된 [[테이블(Table)]]이라는 것을 알 수 있고, 두 [[테이블(Table)]]을 [[JOIN]]할 수 있다.

- 아래 쿼리(Query)문은 country 테이블과 city [[테이블(Table)]]을 생성한다.

```sql
CREATE TABLE country (  
	country_id integer,  
	name varchar(50),  
	population integer
);  
 
CREATE TABLE city (  
	city_id integer,  
	name varchar(50),  
	country_id integer
);
```

- city [[테이블(Table)]]의 country_id [[열(Column)]]을 주목하자.
- 이 [[열(Column)]]은 country [[테이블(Table)]]의 country_id 와 같은 [[열(Column)]]이다.
- 이 [[열(Column)]]이 두 [[테이블(Table)]]을 이어주는 다리 역할을 한다.

- city 테이블의 country_id 값은 country 테이블의 country_id 값과 1:1 매칭이 되어야 하며, country 테이블의 country_id 열 내에 존재하지 않는 값이 city 테이블의 country_id 열 내에 존재할 수는 없다.

- city 테이블은 country 테이블의 값을 공유받는 자식과 같은 존재이기 때문에, country 테이블의 country_id 안에 존재하지 않는 값이 city 테이블의 country_id 안에 존재할 수 없는 것이다.

- 사람인 우리는 두 [[테이블(Table)]]이 서로 연관성([[연관 관계(Relationships)]])이 있다는 것을 알 수 있지만, [[데이터베이스(DataBase)]]는 그러하지 못한다.
- 게다가, 우리가 아무런 제한을 걸지 않는 이상 city 테이블의 country_id 열에 country 테이블의 country_id [[열(Column)]]에 존재하지 않는 값을 넣고자 한다면 충분히 넣을 수 있다.

- 따라서 우리는 외래 키 제한 표현(Foreign Key Constraint)을 사용해 데이터베이스에게 두 테이블 간의 관계를 알려주고, 부모 테이블(country 테이블)에 존재하지 않는 값을 자식 테이블(city 테이블)에서 만들려고 하는 불상사를 막을 수 있다.


### 두 테이블 간의 관계를 정의

- 외래 키 제한 표현의 가장 첫 번째 역할은 두 테이블 간의 관계를 정의하는 일이다.

- 위의 예시에서 city 테이블에 외래 키를 생성하여 country 테이블과 city 테이블 간의 관계를 정의해 줄 건데, 외래 키를 생성하기 전 먼저 해야 할 일은 기본 키를 정의하는 것이다.
- 아래 쿼리문을 통해 기본 키나 외래 키가 존재하지 않았던 맨 처음의 테이블을 없앤 후 새로이 기본 키와 외래 키를 정의한 테이블을 재생성하도록 한다.

1. country 테이블에 [[기본 키(Primary Key)]] 생성한다.

```sql
DROP TABLE country;  

CREATE TABLE country(  
	country_id integer,  
	name varchar(50),  
	population integer,  
	PRIMARY KEY (country_id) 
); 
```

2. [[기본 키(Primary Key)]]와 외래 키를 정의한 새로운 [[테이블(Table)]] 생성한다.

```sql
DROP TABLE city;  

CREATE TABLE city (  
	city_id integer,  
	name varchar(50),  
	country_id integer,  
	PRIMARY KEY (city_id),  
	FOREIGN KEY (country_id) REFERENCES country(country_id) 
);
```

- [[외래 키(Foreign Key)]]를 생성하고 나면, 두 [[테이블(Table)]]을 연결할 수 있는 다리를 놓아준 것뿐만 아니라 country [[테이블(Table)]]의 country_id [[열(Column)]]에 존재하지 않는 값이 city [[테이블(Table)]]의 country_id [[열(Column)]]에 존재하게 되는 불상사를 막을 수 있다.


### 외래 키 제한 표현 이해하기

- 두 [[테이블(Table)]] 사이에 외래 키가 존재하게 되면, 더 넓은 정보를 담고 있는 테이블은 부모 테이블이라고 보고 그보다 좁은 정보를 담고 있는 [[테이블(Table)]]을 자식 테이블이라고 본다.

- 부모 테이블과 자식 테이블 간의 외래 키가 항상 가지는 규칙을 기억하자.

- 자식 테이블 안의 값이 부모 테이블 안에 언제나 존재해야 한다.
- 그렇지 않으면 오류가 발생한다.

- 아래는 예시이다.
- 빈 테이블뿐이라 테이블 안에 같은 값들을 집어넣으려고 한다.

- 아래 쿼리(Query)문은 아무런 문제 없이 정상적으로 실행될 것이다.

- 그 이유는, 자식 테이블에 새로 넣어준 값이 부모 테이블에 존재하기 때문이다.

- city 테이블에 새로 넣은 값인 뉴욕은 국가 아이디가 1(미국)인데, 미국은 이미 위의 쿼리를 통해 생성된 값이기 때문이다.

```sql
INSERT INTO country (country_id, name, population) VALUES (1, 'United States', 328000000);
-- _Query returned successfully: one row affected_  
   
INSERT INTO city (city_id, name, country_id) VALUES (100, 'New York', 1);
-- _Query returned successfully: one row affected_
```

- 하지만, 부모 테이블에 없는 값을 자식 테이블에만 집어넣고자 한다면 쿼리는 실행되지 않다.
- 그리고 아래와 같은 오류 메시지를 보여줄 것이다.

```sql
INSERT INTO city (city_id, name, country_id) VALUES (101, 'Paris', 2);  
 
-- ERROR: Insert on table <<city>> violates foreign key <<fk_city_country>>  
-- DETAIL: The key (country_id)=(2) is not present in the table <<country>>.
```

- 지금까지는 부모 테이블과 자식 테이블에 새로운 값을 넣는 상황만 다루어 보았다.

- 이번에는 역으로 삭제하는 상황을 가정한다.
- 어떤 값을 부모 테이블에서 지웠는데, 자식 테이블에선 그 값이 지워지지 않는다면 해당 데이터의 일관성은 유지되지 못할 것이다.

- 이런 데이터를 활용한 분석은 정확하지도 않다.
- 만약, 외래 키를 정의할 때 ON DELETE CASCADE 절을 추가로 적어준다면 이런 일이 발생하는 것을 막을 수 있다. 

- 참고로, ON DELETE CASCADE 이외에도 외래 키를 정의할 때 사용되는 표현으로는 RESTRICT, NO ACTIONS, SET NULL 등이 있다.

- 그럼 방금 설명한 CASCADE DELETE 표현을 직접 사용하여 테이블을 생성해보도록 한다.

```sql
DROP TABLE city;  

CREATE TABLE city (  
	city_id integer,  
	name varchar(50),  
	country_id integer,  
	PRIMARY KEY (city_id),  
	FOREIGN KEY (country_id) 
	REFERENCES country(country_id) ON DELETE CASCADE
);
```

- 몇 마디를 추가로 적어줌으로써 부모 테이블에서 특정 값이 삭제되면, 자동적으로 자식 테이블에서도 그 값이 지워지도록 설정할 수 있다.

- 이를 통해 [[데이터베이스(DataBase)]] 내의 데이터의 퀄리티를 유지할 수 있겠다.


### 외래 키가 여러 개의 열로 존재할 수 있을까?

- 한 테이블 내에서 여러 개의 열이 기본 키가 될 수 있는 것처럼, 외래 키도 여러 개의 열이 외래 키가 될 수 있다.

- 예시를 통해 여러 개의 외래 키를 정의해보도록 한다.

- 어느 명품 시계점은 본인들이 생상하는 시계의 종류를 회사 데이터베이스에 저장하고 있다.

- 손목시계의 종류는 시계마다의 대 모델명(model)과 소 모델명(sub_model)으로 식별할 수 있다.

- 이는 즉, 시계의 대 모델명과 소 모델명의 조합으로 새로운 행이 추가될 때마다 각 행들이 고유한 값을 가질 수 있도록 돕는 기본 키로 사용될 수 있다.
- 테이블이 어떤 식으로 구성이 되어 있는지 직접 보시죠.

- 첫 번째 테이블의 이름은 watch이고 [[기본 키(Primary Key)]]는 model, sub_model이다.

| model    | sub-model | price  |
| -------- | --------- | ------ |
| Exact-24 | zafiro    | $19200 |
| Exact-24 | diamond   | $23000 |
| Classic  | Army      | $9200  |
| Classic  | City      | $7500  |

- 그런데, 해당 데이터베이스에는 watch 말고도 다른 테이블이 존재한다고 하자.

- 바로 owner 테이블이다.
- 이 테이블은 시계의 주인(owner name), 고객이 시계를 구매한 날짜(purchase_date), 구매한 시계의 대모델명(model), 시계의 소 모델명(sub-model)에 관한 정보를 저장하고 있다.

- 해당 테이블은 시계를 사간 '고객'에 관한 테이블이기 때문에 고객 이름인 owner_name 이 [[기본 키(Primary Key)]]가 되고, 그들이 사간 시계에 관한 정보인 watch 테이블과 동일한 값을 공유하는 model과 sub_model 이 외래 키가 된다.

- 이를 통해, 여러 개의 외래 키를 정의하는 게 가능하다는 것을 살펴볼 수 있다.

- 두 번째 테이블 이름은 owner  이고 [[기본 키(Primary Key)]]는 owner_name, 외래키는 model, sub_model이다.

| owner name   | model    | sub-model | purchase_date |
| ------------ | -------- | --------- | ------------- |
| Elena Doe    | Exact-24 | Zafiro    | 12/23/2003    |
| Mary Smith   | Exact-24 | Zafiro    | 09/12/2006    |
| Zaira Bynz   | Exact-24 | Diamond   | 09/09/2009    |
| John Wats    | Classic  | Army      | 09/01/2011    |
| Mario Zin    | Classic  | Army      | 07/11/2011    |
| Susan Bod    | Exact-24 | Diamond   | 06/10/2015    |
| Marian Doyle | Classic  | City      | 03/12/2019    |

- 아래는 지금까지 이야기한 watch 테이블과 owner 테이블에 여러 개의 기본 키 및 외래 키를 생성할 쿼리(Query)문이다.

```sql
-- 1. 부모 테이블 watch 테이블에 다수의 기본 키 생성
CREATE TABLE watch(  
	model varchar(15),  
	sub_model varchar(15),  
	price numeric,  
	PRIMARY KEY (model, sub_model) 
);  
   
-- 2. 자식 테이블 owner 테이블에 다수의 외래 키 생성
CREATE TABLE owner(  
	owner_name varchar(5),  
	model varchar(15),  
	sub_model varchar(15),  
	purchase_date date,  
	PRIMARY KEY (owner),  
	FOREIGN KEY (model, sub_model) REFERENCES watch(model, sub_model) 
);
```