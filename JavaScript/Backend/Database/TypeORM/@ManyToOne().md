- [[TypeORM]]에서 [[다대일(ManyToOne)]] [[연관 관계(Relationships)]]를 나태낼 수 있는 [[데코레이터(Decorator)]]이다.

- A [[테이블(Table)]]의 [[행(Row)]](데이터) 여러개와 B [[테이블(Table)]]의 [[행(Row)]](데이터) 하나가 연결되는 관계이다.
- 사실상 [[@OneToMany()]]와 세트이다.

- @ManyToOne()의 경우 관계정의가 필수적이지만, [[@OneToMany()]]의 경우 생략이 가능하다.
- 연관관계의 주인이므로 @ManyToOne()가 정의된 [[엔티티(Entity)]]의 [[테이블(Table)]]의 경우, 추가 필드([[열(Column)]])가 생긴다.


## 문법

- 아래 문법을 사용했을 때, N(다)이 있는 쪽이 [[연관 관계(Relationships)]]의 주인이므로, "상대 테이블 이름_id" 형식의 네이밍 필드가 생긴다.

```ts
@ManyToOne(() => Entity, (entity) => entity.id, { eager: true })  
entity: Entity;
```

### Type

- 첫번째 [[매개변수(parameter)]]는 타입을 반환하는 [[함수(Function)]]를 입력한다.
- 즉, [[JOIN]]할 [[엔티티(Entity)]]를 설정한다.
### inverseSide

- 두번째 [[매개변수(parameter)]]는 첫번째 [[매개변수(parameter)]]에 입력한 [[클래스(class)]]의 필드([[열(Column)]]) 중 하나를 입력한다.
- 이 필드([[열(Column)]])은 서로 관련지을 [[속성(Property)]]이여야 한다.
- 즉, 반대쪽 [[엔티티(Entity)]]를 설정한다.
### Option

- 여러가지 옵션을 선택한다. 
- 예를 들어, eager이 true일 때는 user 정보를 가져올 때, board도 같이 가져온다.

## 예시

- 아래 코드와 같이 photo [[테이블(Table)]]에 user_id 필드([[열(Column)]])이 생성되며 user [[테이블(Table)]]과 [[연관 관계(Relationships)]]가 형성된다.

```ts
@Entity()
export class Photo {
	@PrimaryGeneratedColumn()
	id: number;
	
	@Column()
	url: string;
	
	@ManyToOne(() => User, (user) => user.photos)
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
	
	@OneToMany(() => Photo, (photo) => photo.user)
	photo: Photo[];
}
```