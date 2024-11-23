- [[TypeORM]]은 QueryRunner를 사용하여 [[트랜잭션(Transaction)]]을 실행할 수 있다.

- 쿼리러너를 사용하기 위해서는 [[DataSource]] [[객체(Object)]]를 [[초기화(initialization)]]해야 한다.
- [[DataSource]]는 따로 [[의존성 주입(Dependency Injection)]]이 필요하지 않는다.


## QueryRunner 사용 과정

1. QueryRunner를 생성하고, connect()를 통해 연결한 뒤, startTransaction()을 통해 [[트랜잭션(Transaction)]]을 시작한다.
2. 수행하여야 할 [[쿼리(Query)]]들을 실행하고, 모든 [[쿼리(Query)]]들이 수행되었을 때엔 commitTransaction(), 에러가 발생되었을 때엔 rollbackTransaction()을 통해 제어한다.
3. 이후에는 release()를 통해 QueryRunner를 해제한다.

- 추가적으로 2번 과정에서 QueryRunner를 사용할 때 아래 예시 코드처럼 [[try catch]] finally 구문까지 확실하게 사용을 해주는 것이 좋다.
- [[createQueryRunner()]] [[메서드(Method)]]를 통해 qr [[인스턴스(Instance)]]를 만들어서 사용한다.

```ts
const qr: QueryRunner = this.dataSource.createQueryRunner();  
await qr.connect();  
await qr.startTransaction();  
  
try {  
	// 로직
	await qr.commitTransaction();
	// 로직
	// 이 상황부터는 qr 인스턴스 사용대신 리포지토리를 사용해도 됨
} catch (e) {  
	// 로직
    await qr.rollbackTransaction();  
} finally {  
    await qr.release();
}
```

- 만약 [[트랜잭션(Transaction)]]을 통한 [[TypeORM]]의 [[FindOptions]]를 사용하기 위해서는 아래와 같이 qr [[인스턴스(Instance)]]의 manger에 정의된 [[FindOptions]] [[메서드(Method)]]들에서 첫번째 [[매개변수(parameter)]]에 [[엔티티(Entity)]]를 넣어주면 그대로 사용가능하다.

```ts
// DataSource가 알 수 있도록 첫번째 매개변수에 엔티티를 넣어줌
const director = await qr.manager.findOne(Director, {  
    where: { id: createMovieDto.directorId }, 
});
```