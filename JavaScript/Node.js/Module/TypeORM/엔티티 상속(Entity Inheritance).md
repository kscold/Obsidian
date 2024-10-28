- [[TypeORM]]에서는 [[엔티티(Entity)]]도 [[클래스(class)]]이기 때문에, [[extends]] [[키워드(Keyword)]]를 이용한 [[상속(Inheritance)]]이 가능하다.


## 예시

- 아래는 정적 클래스로 선언된 [[클래스(class)]] Content이다.

```ts
export abstract class Content { // 정적 클래스로 선언
	@PrimaryGeneratedColum()
	id: number;
	
	@Column(() => Name)
	name: Name; // Name 엔티티를 임베딩함
	
	@Column()
	isActive: boolean;
}
```

- 아래 코드는 Content [[클래스(class)]]를 [[상속(Inheritance)]]한 [[엔티티(Entity)]]이다.

```ts
@Entity()
export class Photo extends Content {
	@Column()
	size: string;
}
```

```ts
@Entity()
export class Post extends Content {
	@Column()
	viewCount: number;
}
```