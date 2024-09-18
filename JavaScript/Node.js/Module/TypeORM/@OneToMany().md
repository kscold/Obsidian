- [[TypeORM]]에서 [[일대다(OnetoMany)]] 관계를 정의하는 [[데코레이터(Decorator)]]이다.

- 하나의 객체가 여러개의 객체를 소유하는 것을 말한다.

- [[@ManyToOne()]]의 경우 관계정의가 필수적이지만, @OneToMany()의 경우 생략이 가능하다.


## 문법


```ts
@OneToMany((type) => entity, (board) => board.user, { eager: true })  
entitys: entity[];
```

### Type

- [[JOIN]]할 [[엔티티(Entity)]]를 설정한다.
### inverseSide

- 반대쪽 [[엔티티(Entity)]]를 설정한다.

### Option

- 여러가지 옵션을 선택한다. 
- 예를 들어, eager이 true일 때는 user 정보를 가져올 때, board도 같이 가져온다.


