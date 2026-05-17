- [[TypeORM]]에서는 [[엔티티(Entity)]]를 선언할 때, [[@Column()]] [[데코레이터(Decorator)]]를 사용하여 엔티티를 임베딩하여 사용할 수 있다.

- 이때 임베딩이 되게 되면 그 임베딩된 [[테이블(Table)]]에 임베드한 [[테이블(Table)]]의 필드([[열(Column)]])들이 생기게 된다.
- 즉, [[엔티티(Entity)]]의 [[속성(Property)]]이 생기게 된다.


## 예시

- 아래는 두 개의 [[속성(Property)]]을 가지고 있는 Name [[테이블(Table)]]이다.

```ts
export class Name {
	@Colum()
	first: string;
	
	@Colum()
	last: string;
}
```

- 아래는 두 개의 [[속성(Property)]]을 가지고 있는 Name [[테이블(Table)]]이다.

```ts
@Entity()
export class User {
	@PrimaryGeneratedColum()
	id: number;
	
	@Column(() => Name)
	name: Name; // Name 엔티티를 임베딩함
	
	@Column()
	isActive: boolean;
}
```

```ts
@Entity()
export class Employee {
	@PrimaryGeneratedColum()
	id: number;
	
	@Column(() => Name)
	name: Name; // Name 엔티티를 임베딩함
	
	@Column()
	salary: number;
}
```

- 아래 이미지와 같이 Name [[엔티티(Entity)]]의 [[속성(Property)]]들이 User와 Employee [[엔티티(Entity)]]의 [[속성(Property)]]으로 들어가게 된다.

![[Pasted image 20241028224823.png]]