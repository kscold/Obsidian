- 쿼리빌더(QueryBuilder)는 [[TypeORM]]의 강력한 기능들 중 하나로, 명쾌하고 편리한 구문을 이용해 [[SQL(Structured Query Language)]] [[쿼리(Query)]]들문을 작성하여, 자동적으로 실행하고 변형된 [[엔티티(Entity)]]들을 자동적으로 얻을 수 있도록 하는 기능이다. 

- 복잡하지 않은 일반적인 [[쿼리(Query)]]를 실행할 때에는 [[리포지토리(Repository)]]를 사용하는게 편리하다.
- 조금 더 복잡한 [[쿼리(Query)]]를 실행해야 하거나 다이나믹하게 [[쿼리(Query)]]를 만들어 가야 할 경우 쿼리빌더(QueryBuilder)를 사용해야 한다.

- [[createQueryBuilder()]] [[메서드(Method)]]로 쿼리빌더(QueryBuilder)를 생성한다.


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

- [[데이터베이스(DataBase)]] 관계가 복잡해질 수록 기존 [[리포지토리(Repository)]]를 이용한 [[메서드(Method)]]들을 통한 [[CRUD]] 사용이 어려워지는 경우가 존재한다고 한다.  

- 예를들어 [[일대다(OnetoMany)]] 관계의 user, board [[연관 관계(Relationships)]]에 comment 댓글 기능을 추가한다고 가정한다.
- user와 board와 [[다대일(ManyToOne)]] 관계를 맺는 comment entity가 추가되었을 때, 관련된 userRepo.save(), boardRepo.save()가 오작동하게 되며, query builder로 refactoring을 해야 하는 경우가 생긴다.
- 따라서 [[연관 관계(Relationships)]]를 맺은 특정 [[테이블(Table)]]을 조회하기 위해선 직접 query builder로 [[쿼리(Query)]]를 작성해야 한다.  
  
- 결론은, 일부 [[쿼리(Query)]]는 [[ORM(Object Relational Mapping)]] 작업으로 표현할 수 없다.
- 이러한 복잡한 [[쿼리(Query)]]는 직접 [[SQL(Structured Query Language)]]문으로 작성하는 작업으로 회귀해야 한다.
- 예를 들어 밑의 [[SQL(Structured Query Language)]] 같이, 쿼리 내에 서브 쿼리가 포함된 경우 혹은 [[테이블(Table)]] 간 [[JOIN]]이 필요한 경우, [[ORM(Object Relational Mapping)]]으로 명확하게 표현할 수 없다.  

```sql
select * from users
	where id not in (select user_id from boards where board_id = 2)
	and id in (select user_id from boards where board_id = 3);
```

  
- 프레임워크 별로, 또 개발 시스템별로 다양한 [[ORM(Object Relational Mapping)]]이 존재한다.
- 종류와 가짓수가 정말 많다.
- 모든 [[ORM(Object Relational Mapping)]]을 이해하고 사용하는 것은 쉬운 일이 아니다.  

- 따라서 [[SQL(Structured Query Language)]]을 명확히 이해하고 [[데이터베이스(DataBase)]]를 다루는 기본적인 능력을 길러야한다.
- [[쿼리(Query)]]를 손수 작성하는 방법도 있지만, [[SQL(Structured Query Language)]] [[쿼리(Query)]]에 대한 이해를 바탕으로, QueryBuilder을 사용하도록 하자.


## QueryBuilder을 사용시 주의점

- QueryBuilder을 사용할 때, [[QueryBuilder.where()]]절을 표현에서 유일한 [[매개변수(parameter)]]을 제공해야 한다. 

- 다음은 user 테이블과 pk로 연결된 linkedSheep, linkedCow 테이블을 조인하는 query builder 사용 예시이다.  
- 서로 다른 [[매개변수(parameter)]]에 대해, id를 두번 사용하는 일이 없어햐 한다.
- unique하게 지정된 sheepId, cowId로 [[JOIN]]해야 한다.

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


## QueryBuilder 생성

- QueryBuilder을 이용해 개발 환경 내에서 [[쿼리(Query)]]를 생성하기 위해서는 [[createQueryBuilder()]] [[메서드(Method)]]를 사용한다.


## QueryBuilder로 값 얻기

- 하나의 데이터([[행(Row)]])만 얻으려면 [[QueryBuilder.getOne()]] [[메서드(Method)]]를 사용한다.
- 반대로 복수로 얻고싶다면 [[QueryBuilder.getMany()]]를 사용하면 된다.


## QueryBuilder의 유형

- QueryBuilder는 서브쿼리([[QueryBuilder.subQuery()]])를 사용할 수 있다.

### 1. SelectQueryBuilder

- [[SELECT]]문 [[쿼리(Query)]]를 생성하고 실행할 때 사용한다.
- [[SELECT]]문의 경우 [[QueryBuilder.getOne()]] [[메서드(Method)]]로 실행시킨다.

```ts
const movie = await dataSource
    .createQueryBuilder()
    .select("movie")
    .from(Movie, "movie") // movie라는 alias를 설정할 수 있음
    .leftJoinAndSelect("movie.detail", "detail") // left Join 방법
    .leftJoinAndSelect("movie.director", "director")
    .leftJoinAndSelect("movie.genres", "genres")
    .where("movie.id = :id", { id: 1 })
    .getOne();
```

### 2. InsertQueryBuilder

- [[INSERT]]문 [[쿼리(Query)]]를 생성하고 실행할 때 사용한다. 
- [[QueryBuilder.execute()]]를 사용해서 [[쿼리(Query)]]를 실행시킨다.

```ts
await dataSource
    .createQueryBuilder()
    .insert()
    .into(Movie)
    .values([
        { 
		    title: "New Movie",
		    genre: "Action",
		    director: director,
		    genres: genres
	    }
    ])
    .execute();
```

### 3. UpdateQueryBuilder

- [[UPDATE]]문 [[쿼리(Query)]]를 생성하고 실행할 때 사용한다. 
- [[QueryBuilder.execute()]]를 사용해서 [[쿼리(Query)]]를 실행시킨다.

```ts
await dataSource
    .createQueryBuilder()
    .update(Movie)
    .set({ title: "Updated Title", genre: "Drama" })
    .where("id = :id", { id: 1 })
    .execute();
```

### 4. DeleteQueryBuilder

- [[DELETE]]문 [[쿼리(Query)]]를 생성하고 실행할 때 사용한다. 
- [[QueryBuilder.execute()]]를 사용해서 [[쿼리(Query)]]를 실행시킨다.

```ts
await dataSource
    .createQueryBuilder()
    .delete()
    .from(Movie)
    .where("id = :id", { id: 1 })
    .execute();
```

### 5. RelationQueryBuilder

- [[연관 관계(Relationships)]] 지어서 수행하는 [[쿼리(Query)]]를 만들고 실행할 때 사용한다.

```ts
const genres = await dataSource
    .createQueryBuilder()
    .relation(Movie, "genres") // Moview 테이블의 genres를 가져옴
    .of(1) // Movie 테이블의 id
    .loadMany(); // 연관 테이블의 테이블을 가져옴
```