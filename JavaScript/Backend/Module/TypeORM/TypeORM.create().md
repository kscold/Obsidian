- [[TypeORM]]은 엔티티 객체 생성 create [[메서드(Method)]]를 통해 [[엔티티(Entity)]] [[객체(Object)]]를 생성할 수 있다.

- 해당 메서드는 [[비동기(asynchronous)]]적으로 동작하지 않는다.

- create 메서드는 [[데이터베이스(DataBase)]]로 [[쿼리(Query)]]문을 발생하지 않는다.
- 순수하게 [[객체(Object)]]만 생성하는 팩터리 메서드이다.
- 즉. [[TypeORM.save()]]는 하지 않는다.


## 예시

- 아래 코드처럼 추가적인 [[엔티티(Entity)]]를 생성하지 않을 수 있다.
- 추가적으로 [[TypeORM.insert()]]나 [[TypeORM.save()]]를 이용해 [[데이터베이스(DataBase)]]에 정보를 삽입할 수도 있다.

```js
const user = AppDataSource.manager.create(User, { id: 1, name: 'm', age: 30 }); 

console.log(user);
```

​