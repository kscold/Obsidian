- ORM(Object Relational Model)은 사물을 추상화시켜 이해하려는 OOP적 사고방식과 DataModel을 정형화하여 관리하려는 RDB([[관계형 데이터베이스(Relational DataBase)]]) 사이를 연결할 계층의 역할로 제시된 패러다임으로 RDB의 모델을 OOP에 엔티티(Entity) 형태로 투영시키는 방식을 사용한다.

- 즉, [[객체(Object)]]와 [[데이터베이스(DataBase)]]간에 연결 역할을 해주는 것이 ORM이다.

[![image](https://kyungyeon.dev/static/4a69d4f1aa838a697a8ff27949a02dd0/f058b/img_orm.png "image")]


## ORM의 등장배경

- ORM을 사용하지 않을 경우 개발자가 개발을 위하여 [[데이터베이스(DataBase)]]에 접근하기 위해서는 [[SQL]] [[쿼리(Query)]]문을 직접 만들었다.

- [[SQL]]문법을 알고 있어야하는 문제가 있었다.
- 쿼리문 작성시간이 오래걸려 개발이 지체되는 문제가 발생했다.

- 개발 코드와 [[데이터베이스(DataBase)]]가 서로 종속된다.

- 띠라서 위와 같은 문제점들이 생겨나기 시작했다. 
- 이러한 문제점을 해결하기 위해 ORM이 등장하였고 ORM이 등장함으로서 SQL Query 문법에서 비교적 자유로워졌으며 개발 코드와 [[데이터베이스(DataBase)]]를 분리함으로써 객체지향의 의도에 부합하게 되었다.


## ORM사용의 장점과 단점

### 장점

#### 1. 개발의 생산성 향상

- 선언문, 할당, 종료 등과같은 부수적인 코드가 사라짐으로써 개발의 생산성 향상되었다.

#### 2. 명확한 [[쿼리(Query)]] (버그와 실수 감소)

- 코드가 간결하고 직관이기 때문에 가독성을 높여준다.

#### 3. [[데이터베이스(DataBase)]] 의존성 감소

- 대부분 ORM 솔루션이 특정 [[데이터베이스(DataBase)]]에 종속적이지 않다.  
- 개발자는 [[데이터베이스(DataBase)]]에 상관없이 [[객체(Object)]]에만 집중할 수 있다.

### 단점

#### 1. 튜닝의 어려움

- 미세한 수정 및 디버그하기가 상대적으로 어렵다.

#### 2. 복잡한 쿼리작성의 문제

- ORM이 지정해 놓은 명령만 내릴 수 있다.

#### 3. Raw쿼리에 비해 느린 실행속도

- DB에 직접 명령을 내리지 않고 한단계 거쳐가기 때문에 조금 느리다.

## Raw [[SQL]] vs TypeORM vs [[시퀄라이즈(Sequelize)]] vs Knex

- 다음은 각각의 문법의 비교이다.

```javascript
// 1. SQL
const query = "SELECT * FROM post WHERE authorId = 12 AND status = 'active'";

// Knex
knex("post").select()
	.where({ authorId: 12, status: 'active'});

// Sequelize
models.Post.findAll({
    where: { authorId: 12, status: 'active' }
})

// TypeORM
connection.getRepository(Post).find({ where: { authorId: 12, status: 'active' } })
```

```javascript
// 2. SQL
const query = "select * 
               from category_page 
               where category_id = 5
                 and (show_at is null or show_at >= now()) 
                 and (hide_at is null or hide_at <= now())
               order by updated_at desc 
               limit 1";
             
// Knex
knex("category_page")
      .where({ category_id: 5 })
      .where(b => b.whereNull('show_at').orWhere('show_at', '>=', moment(now).toDate()))
      .where(b => b.whereNull('hide_at').orWhere('hide_at', '<=', moment(now).toDate()))
      .orderBy('updated_at', 'desc')
      .first()

// Sequelize
models.CategoryPage.findOne({
    where: {
        category_id: 5,
        show_at: {
            [Op.or]: [
                { [Op.eq]: null },
                { [Op.gte]: now }
            ]
        },
        hide_at: {
            [Op.or]: [
                { [Op.eq]: null },
                { [Op.lte]: now }
            ]
        }
    },
    order: [['updated_at', 'DESC']]
})
// 참고 EQ("="), GTE(">="), GT(">"), LT("<"), LTE("<=");

// TypeORM
connection.getRepository(CategoryPage)
    .createQueryBuilder()
    .where("category_id = :categoryId", { categoryId: 5 })
    .andWhere("(show_at is null or show_at >= now())" )
    .andWhere("(hide_at is null or hide_at <= now())" )
    .orderBy("updated_at", "DESC")
    .limit(1)
    .getMany()
```