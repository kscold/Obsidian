- [[TypeORM]]에서는 [[엔티티(Entity)]]를 선언할 때, 공유하고 싶은 [[속성(Property)]]이 있을 때, 사용할 수 있는 기법이다.

- 부모 [[엔티티(Entity)]]는 @TableInheritance() [[데코레이터(Decorator)]]를 이용하여 사용한다.
- [[상속(Inheritance)]]받는 자식 [[엔티티(Entity)]]는 @ChildEntity() [[데코레이터(Decorator)]]를 사용한다.

- 즉, 공통로직으로 [[엔티티(Entity)]]를 묶을 때 좋으나, 성질이 다른 경우 [[연관 관계(Relationships)]]가 다른 [[엔티티(Entity)]]로 나누는 것이 더 좋은 방법이다.


## 예시

```ts
@Entity()
@TableInheritance({
	column: {
		type: "varchar",
		name: "type",
	}
})
export class Content {
	@PrimaryGeneratedColum()
	id: number;
	
	@Column()
	title: string 
	
	@Column()
	description: string;
}
```

- 아래 코드는 Content [[클래스(class)]]를 [[상속(Inheritance)]]한 [[엔티티(Entity)]]이다.

```ts
@ChildEntity()
export class Photo extends Content {
	@Column()
	size: string;
}
```

```ts
@ChildEntity()
export class Post extends Content {
	@Column()
	viewCount: number;
}
```

- 아래와 같이 다른 [[엔티티(Entity)]]로 정의하였지만 공유되는 [[엔티티(Entity)]]의 [[테이블(Table)]]을 볼 수 있다.

![[Pasted image 20241028232627.png]]

- 또한 @TableInheritance() [[데코레이터(Decorator)]]의 설정의 column: { type: "varchar", name: "type" } 설정 부분을 통해서, 어떤 [[엔티티(Entity)]]에 의하여 생성되었는지 확인이 가능하다.
- 즉, Photo [[엔티티(Entity)]]에 의해서인지, Post [[엔티티(Entity)]]에 의해서 인지 확인할 수 있는 type 필드([[열(Column)]])이 생긴다.