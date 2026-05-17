- [[Spring/SQL/필드(Field)|필드(Field)]] 혹은 [[속성(Attribute)]]을 임베드 가능한 [[클래스(Class)]]로 지정하는데 사용한다.
- 임베드 가능한 클래스란, 고유한 [[기본 키(Primary Key)]]가 없는 관련 [[Spring/SQL/필드(Field)|필드(Field)]] 그룹을 나타내는데 사용된다.

- 일반적으로 [[엔티티(Entity)]] 내에서 관련 열([[Spring/SQL/필드(Field)|필드(Field)]])을 함께 그룹화하는데 사용된다.

- 몇몇 [[Spring/SQL/필드(Field)|필드(Field)]]를 분리된 [[클래스(Class)]]로 [[캡슐화(encapsulation)]]하는데 이용된다.

- [[엔티티(Entity)]]와 함께 컬럼([[Spring/SQL/필드(Field)|필드(Field)]])으로 직접 매핑된다.

- [[JPA(Java Persistence API)]]에서는 [[엔티티(Entity)]] 내부의 값을 더 응집시켜 [[객체(Object)]]로 데이터를 표현한다.

-  따라서 이때 @Embedded와 @Embeddable [[어노테이션(Annotation)]]을 사용한다.
- @Embeddable과 @Embeddaed를 함께 사용한다.

- [[클래스(Class)]]를 @Embeddable로 표시하면 이 [[클래스(Class)]]가 [[데이터베이스(DataBase)]]에 별도로 [[테이블(Table)]]로 존재하지 않음을 명시한다.

 - Embeddable 클래스는 클래스에 포함될 수 있다.
 - Embedded 클래스는 이제 클래스에 [[Spring/SQL/필드(Field)|필드(Field)]]로 포함된다.


## @Embeddable

- 회원가입을 생각하여 간단한 코드로 확인해 봅니다.

```java
@Entity
@Table(name = "user")
public class User {
	
    @Id
    @GeneratedValue(strategy = GenerationType.Identify)
    @Column(name = "user_id")
    private Long id;
    private String name;
    private String phoneNum;
    
    private String zipCode;
    private String address;
    private String addressDetail;
}
```

- 위의 [[엔티티(Entity)]]를 보면 User의 정보중에 "주소"를 표현하는 칼럼([[Spring/SQL/필드(Field)|필드(Field)]])들이 있다.  
- 여기서 "주소"라는 [[객체(Object)]]로 묶어서 관리하면 조금더 깔끔하지 않을까? 라는 생각이 들때가 있다.

- 이때 여기서 사용할 수 있는 [[어노테이션(Annotation)]]이 @Embedded와 @Embeddable [[어노테이션(Annotation)]]이다.

```java
@Entity
@Table( name = "user")
public class User {
	
    @Id
    @GeneratedValue(strategy = GenerationType.Identify)
    @Column( name ="user_id")
    private Long id;
    private String name;
    private String phoneNum;
    
    @Embedded
    private Address address;
}
```

```java
@Embeddable
public class Address{
    private String zipCode;
    private String address;
    private String addressDetail;
}   
```

- 이렇게 [[클래스(Class)]]를 나누어 각각 해당 [[어노테이션(Annotation)]]을 붙여주면 되는데 @Embedded는 생략이 가능하다.
- 결과적으로 아래 사진처럼 컬럼들이 잘 생긴 것을 알 수 있다.

![](https://velog.velcdn.com/images/jyyoun1022/post/8e2a9e73-d24d-497a-b58d-bc305b510557/image.png)
