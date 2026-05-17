- [[TypeORM]]에서 [[일대일(OneToOne)]] [[연관 관계(Relationships)]]를 나태낼 수 있는 [[데코레이터(Decorator)]]이다.

- A [[테이블(Table)]]의 [[행(Row)]](데이터) 하나와 B [[테이블(Table)]]의 [[행(Row)]](데이터) 하나가 연결되는 관계이다.

- @OneToOne()은 두 [[테이블(Table)]] 중 누가 [[연관 관계(Relationships)]]를 들고 있어도 상관이 없기 때문에, 어떤 [[테이블(Table)]]이 [[연관 관계(Relationships)]]를 들고 있을지 명시해줘야 한다.

- 따라서 @JoinTable [[데코레이터(Decorator)]]를 통해서 어떤 [[속성(Property)]]이 [[연관 관계(Relationships)]]의 주인이 될지 정해 줄 수 있다.
- @JoinTable의 경우 꼭 한쪽에만 적용해야한다.
- 둘 모두 적용하는 것은 불가능하다.


## 예시

```ts
@Entity()
export class Profile {
	@PrimaryGeneratedColumn()
	id: number;
	
	@Column()
	gender: string;
	
	@Column()
	photo: string;
	
	@OneToOne(() => User, (user) => user.profile)
	user: User;
}
```

```ts
@Entity()
export class User {
	@PrimaryGeneratedColumn()
	id: number;
	
	@Column()
	name: string;
	
	@OneToOne(() => Profile)
	@JoinColum()
	profile: Profile;
}
```