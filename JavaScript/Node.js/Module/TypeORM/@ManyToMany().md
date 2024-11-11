- [[TypeORM]]에서 [[다대다(ManyToMany)]] [[연관 관계(Relationships)]]를 나태낼 수 있는 [[데코레이터(Decorator)]]이다.

- A [[테이블(Table)]]의 [[행(Row)]](데이터) 하나와 B [[테이블(Table)]]의 [[행(Row)]](데이터) 여러개가 연결되는 관계이다.
- 사실상 [[@ManyToOne()]]과 세트이다.

- [[@OneToOne()]] [[연관 관계(Relationships)]]와 마찬가지로 @JoinTable [[데코레이터(Decorator)]]를 한쪽에 적용해줘야 한다.
- 중간 [[테이블(Table)]]이 생성될 때 @JoinTable이 적용된 [[테이블(Table)]] 이름이 먼저 위치하게 된다.


## 문법

- N이 서로를 가리키는 [[다대다(ManyToMany)]] [[연관 관계(Relationships)]]이기 때문에, 반대의 [[엔티티(Entity)]]를 인자로 넣어주면 된다.

```ts
@ManyToOne(() => ReverseEntity)  
entity: ReverseEntity; 
```

### Type

- 첫번째 [[매개변수(parameter)]]는 타입을 반환하는 [[함수(Function)]]를 입력한다.
- 즉, [[JOIN]]할 [[엔티티(Entity)]]를 설정한다.


## 예시

```ts
@Entity()
export class Category {
	@PrimaryGeneratedColumn()
	id: number;
	
	@Column()
	name: string;
	
	@ManyToMany(() => Question)
	questions: Question[];
}
```


```ts
@Entity()
export class Question {
	@PrimaryGeneratedColumn()
	id: number;
	
	@Column()
	title: string;
	
	@Column()
	text: string;
	
	@ManyToMany(() => Category)
	@JoinTable()
	categories: Category[];
}
```