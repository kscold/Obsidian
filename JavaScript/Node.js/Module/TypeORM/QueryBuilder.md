- 쿼리빌더(QueryBuilder)는 [[TypeORM]]의 강력한 기능들 중 하나로, 명쾌하고 편리한 구문을 이용해 [[SQL(Structured Query Language)]] 쿼리들문을 작성하여, 자동적으로 실행하고 변형된 [[엔티티(Entity)]]들을 자동적으로 얻을 수 있도록 하는 기능이다. 

- [[createQueryBuilder()]] [[메서드(Method)]]로 쿼리빌더(QueryBuilder)를 사용한다.

## [[SQL(Structured Query Language)]]과 쿼리빌더(QueryBuilder)

- 예를 들어보자면, [[SQL(Structured Query Language)]]문으로 user의 id가 1번인 user의 id, firstName, lastName을 출력하는 [[SQL(Structured Query Language)]]문은 다음과 같다.

```sql
SELECT
	user.id as userId,
	user.firstName as userFirstName,
	user.lastName as userLastName
FROM users user
WHERE user.id = 1
```

- DBMS에서 직접 [[SQL(Structured Query Language)]]문을 작성하지 않고, [[TypeORM]]을 이용해 [[데이터베이스(DataBase)]]에 직접 접근하여 원하는 데이터를 조회하려면 다음과 같이 작성할 수 있다.

```ts
const firstUser = await dataSource
	.getRepository(User)
	.createQueryBuilder("user")
	.where("user.id = :id", { id: 1 })
	.getOne()
```


## QueryBuilder를 사용하는 이유

### ORM의 장점

- [[ORM(Object Relational Mapping)]]이 도입된 것은, 개발 프레임워크 내에서 생성하는 [[객체(Object)]] 혹은 [[모델(Model)]]와 [[데이터베이스(DataBase)]]의 [[연관 관계(Relationships)]]([[테이블(Table)]])를 연결(매핑)을 쉽게 하고, 애플리케이션의 [[객체(Object)]]를 [[데이터베이스(DataBase)]]로 쉽게 migration하는 작업을 편리하게 하기 위함이다.  

- [[ORM(Object Relational Mapping)]]의 장점으로는 중복 코드 방지, 다른 [[데이터베이스(DataBase)]]로 쉽게 교체/마이그레이션 가능, 여러 [[테이블(Table)]]에 쉽게 [[쿼리(Query)]]를 날릴 수 있으며, 인터페이스를 작성하는 시간을 아껴 비즈니스 로직에 집중할 수 있다는 점이 있다.  
### ORM의 단점

- [[데이터베이스(DataBase)]] 관계가 복잡해질 수록 기존 [[리파지터리(Repository)]]를 이용한 [[메서드(Method)]]들을 통한 [[CRUD]] 사용이 어려워지는 경우가 존재한다고 한다.  

- 예를들어 [[일대다(OnetoMany)]] 관계의 user, board [[연관 관계(Relationships)]]에 comment 댓글 기능을 추가한다고 가정한다.
- user와 board와 [[다대일(ManyToOne)]] 관계를 맺는 comment entity가 추가되었을 때, 관련된 userRepo.save(), boardRepo.save()가 오작동하게 되며, query builder로 refactoring을 해야 하는 경우가 생긴다.
- 따라서 [[연관 관계(Relationships)]]를 맺은 특정 [[테이블(Table)]]을 조회하기 위해선 직접 query builder로 [[쿼리(Query)]]를 작성해야 한다.  
  
- 결론은, 일부 쿼리는 ORM 작업으로 표현할 수 없다.
- 이러한 복잡한 [[쿼리(Query)]]는 직접 [[SQL(Structured Query Language)]]문으로 작성하는 작업으로 회귀해야 한다.
- 예를 들어 밑의 [[SQL(Structured Query Language)]] 같이, 쿼리 내에 서브 쿼리가 포함된 경우 혹은 [[테이블(Table)]] 간 [[JOIN]]이 필요한 경우, [[ORM(Object Relational Mapping)]]으로 명확하게 표현할 수 없다.  

```sql
select * from users
where id not in (select user_id from boards where board_id = 2)
and id in (select user_id from boards where board_id = 3);
```

  
- 프레임워크 별로, 또 개발 시스템별로 다양한 ORM이 존재한다.
- 종류와 가짓수가 정말 많다.
- 모든 ORM을 이해하고 사용하는 것은 쉬운 일이 아니다.  

- 따라서 결론은 SQL을 명확히 이해하고 DB를 다루는 기본적인 능력을 기르도록 하자.
- 쿼리를 손수 작성하는 방법도 있지만, SQL 쿼리에 대한 이해를 바탕으로, QueryBuilder을 사용하도록 하자.  
  

## QueryBuilder을 사용시 주의점

- QueryBuilder을 사용할 때, [[WHERE]]절을 표현에서 유일한 [[매개변수(parameter)]]을 제공해야 한다.  
- 다음은 user 테이블과 pk로 연결된 linkedSheep, linkedCow 테이블을 조인하는 query builder 사용 예시이다.  
- 서로 다른 [[매개변수(parameter)]]에 대해, id를 두번 사용하는 일이 없도록 하자.
- unique하게 지정된 sheepId, cowId로 조인하도록!

```ts
// id로 linkedSheep과 linkedCow를 찾으려는 경우 (X)
const result = await dataSource
	.createQueryBuilder('user')
	.leftJoinAndSelect('user.linkedSheep', 'linkedSheep')
	.leftJoinAndSelect('user.linkedCow', 'linkedCow')
	.where('user.linkedSheep = :id', { id: sheepId })
	.andWhere('user.linkedCow = :id', { id: cowId });
    
// sheepId, cowId로, unique로 지정된 이름을 사용 (O)    
const result = await dataSource
	.createQueryBuilder('user')
	.leftJoinAndSelect('user.linkedSheep', 'linkedSheep')
	.leftJoinAndSelect('user.linkedCow', 'linkedCow')
	.where('user.linkedSheep = :sheepId', { sheepId })
	.andWhere('user.linkedCow = :cowId', { cowId });
```


## QueryBuilder 생성하고 사용

- QueryBuilder을 이용해 개발 환경 내에서 쿼리를 생성하기 위해서는 다음과 같이 3가지의 방법이 존재한다.  

### 1. [[DataSource]] 사용

- 직접 user [[테이블(Table)]]을 select() 를 이용해 명시해줘야 한다.

```ts
const user = await dataSource
    .createQueryBuilder()
    .select("user")
    .from(User, "user")
    .where("user.id = :id", { id: 1 })
    .getOne()
```
### 2. entity manager 사용

- select() 대신, [[createQueryBuilder()]] [[매개변수(parameter)]]에 [[엔티티(Entity)]] 이름과, [[테이블(Table)]] 이름을 지정한다.

```ts
const user = await dataSource.manager
    .createQueryBuilder(User, "user")
    .where("user.id = :id", { id: 1 })
    .getOne()
```
### 3. [[리파지터리(Repository)]] 사용

- getRepository()를 이용해 Entity를 지정한 후, [[createQueryBuilder()]]에는 테이블 이름을 지정한다.

```ts
const user = await dataSource
    .getRepository(User)
    .createQueryBuilder("user")
    .where("user.id = :id", { id: 1 })
    .getOne()
```


## QueryBuilder로 값 얻기

- 하나의 데이터만 얻으려면 [[getOne()]] [[메서드(Method)]]를 사용한다.
- 반대로 복수로 얻고싶다면 [[getMany()]]를 사용하면 된다.


## QueryBuilder의 유형

### 1. SelectQueryBuilder

- [[SELECT]]문 [[쿼리(Query)]]를 생성하고 실행할 때 사용한다.

```ts
const user = await dataSource
    .createQueryBuilder()
    .select("user")
    .from(User, "user")
    .where("user.id = :id", { id: 1 })
    .getOne()
```

### 2. InsertQueryBuilder

```ts
await dataSource
    .createQueryBuilder()
    .insert()
    .into(User)
    .values([
        { firstName: "Timber", lastName: "Saw" },
        { firstName: "Phantom", lastName: "Lancer" },
    ])
    .execute()
```

### 3. UpdateQueryBuilder

```ts
await dataSource
    .createQueryBuilder()
    .update(User)
    .set({ firstName: "Timber", lastName: "Saw" })
    .where("id = :id", { id: 1 })
    .execute()
```

### 4. DeleteQueryBuilder

```ts
await dataSource
    .createQueryBuilder()
    .delete()
    .from(User)
    .where("id = :id", { id: 1 })
    .execute()
```

### 5. RelationQueryBuilder

- [[연관 관계(Relationships)]] 지어서 수행하는 [[쿼리(Query)]]를 만들고 실행할 때 사용한다.

```ts
await dataSource
    .createQueryBuilder()
    .relation(User,"photos")
    .of(id)
    .loadMany();
```