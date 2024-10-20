- 모든 [[TypeORM]]의 [[TypeORM.find()]] [[메서드(Method)]]와 관련된 API는 FindOptions를 인자로 받는다.

- FindOptions는 어떤 값들을 불러올지 필터링하는 역할을 한다.
- FindOptions의 정확한 [[타입스크립트(TypeScript)]] [[타입 표기(Type Annotation)]]는 FindOneOptions와 FindManyOptions로 정의 되어있다.

- FindManyOptions는 FindOneOptions를 [[상속(Inheritance)]]받고 skip, take 두가지 [[속성(Property)]]이 더 존재한다.


## FindOptions 정의

- FindOneOptions의 인터페이스([[Interface]])는 아래와 같이 정의되어 있다.

```ts
export interface FindOneOptions<Entity = any> {
	select?: FindOptionsSelect<Entity>
	
	where?: FindOptionsWhere<Entity>[]
	
	relations?: FindOptionsRelations<Entity>
	
	order?: FindOptionsOrder<Entity>
	
	cache?: boolean | number
}
```

### [[FindOptionsSelect]]

- [[FindOptionsWhere]]의 [[속성(Property)]]은 불러올 필드([[열(Column)]])을 지정할 수 있다.
### [[FindOptionsWhere]]

- [[FindOptionsWhere]]의 [[속성(Property)]]은 필터링할 조건을 설정할 수 있다.
### [[FindOptionsRelations]]

- [[FindOptionsRelations]]의 [[속성(Property)]]은 불러올 [[연관 관계(Relationships)]] [[테이블(Table)]]을 지정할 수 있다.
### [[FindOptionsOrder]]

 - [[FindOptionsOrder]]의 [[속성(Property)]]은 정렬을 지정할 수 있다.
### [[FindOptionsCache]]

 - [[FindOptionsOrder]]의 [[속성(Property)]]은 캐싱 기간을 지정할 수 있다.
### [[FindManyOptions]]



## FindOptions를 사용하는 find [[메서드(Method)]]들 

```ts
const rows = await repository.find({
	where: {
		firstName: "Code Factory",
	},
})

const row = await repository.findOne({
	where: {
		firstName: "Code Factory",
	},
})

const [rows, count] = await repository.findAndCount({
	where: {
		firstName: "Code Factory",
	},
})
```
