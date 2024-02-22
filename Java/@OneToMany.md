- @OneToMany [[어노테이션(Annotation)]]을 가지고 있는 쪽이 부모 [[엔티티(Entity)]]가 된다.



## @ManyToOne 속성

- @ManyToOne 속성에는 다음과 같은 것들이 있다.
	- targetEntity
	- cascade
	- fetch
	- orphanRemoval 

### orphanRemoval 

- orphanRemoval 옵션은 @OneToMany에만 존재하는 옵션이다.
- orphanRemoval 옵션은 연관관계가 끊긴 [[엔티티(Entity)]]에 대해서 REMOVE 작업을 진행하고 전파할 지에 대한 옵션이다.

### targetEntity

- @OneToMany가 설정된 [[Spring/SQL/필드(Field)|필드(Field)]]를 보면 모두 `List<T>` 타입을 이용했는데, [[List]] 말고도 [[Set]], [[Map]] 타입도 이용 가능하다.

- `List<T>`가 아닌 그냥 [[List]]로도 타입 선언이 가능한데, 이럴 때에는 targetEntity의 명시가 필요하다.

```java
@OneToMany(targetEntity = Post.class)
private List posts;
```

- 하지만 이런 경우는 권장되지도 않고 많이 사용되지도 않기 때문에 잘 사용하지 않는 옵션이다.
- [[@ManyToOne]]도 설정할 수 있지만 더더욱 필요 없는 옵션이다.