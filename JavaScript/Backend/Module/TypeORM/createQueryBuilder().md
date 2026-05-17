-  createQueryBuilder()는 [[TypeORM]]을 통해 [[SQL(Structured Query Language)]] [[쿼리(Query)]]를 구성하는 데 쓰이는 [[쿼리빌더(QueryBuilder)]]를 생성한다.


## 문법

- 아래와 같이 createQueryBuilder() 이용해 개발 환경 내에서 쿼리를 생성하기 위해서는 다음과 같이 3가지의 방법이 존재한다.  

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

- select() 대신, createQueryBuilder() [[매개변수(parameter)]]에 [[엔티티(Entity)]] 이름과, [[테이블(Table)]] 이름을 지정한다.

```ts
const user = await dataSource.manager
    .createQueryBuilder(User, "user")
    .where("user.id = :id", { id: 1 })
    .getOne()
```
### 3. [[리포지토리(Repository)]] 사용

- getRepository()를 이용해 Entity를 지정한 후, [[createQueryBuilder()]]에는 [[테이블(Table)]] 이름을 지정한다.

```ts
const user = await dataSource
    .getRepository(User)
    .createQueryBuilder("user")
    .where("user.id = :id", { id: 1 })
    .getOne()
```