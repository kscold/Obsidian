- 데이터베이스 인덱스는 추가 쓰기 및 저장 공간을 희생하여 [[테이블(Table)]]에 대한 데이터 검색 작업의 속도를 향상시키는 데이터 구조이다 .
- 대부분 단일 테이블에서 선택한 데이터 열의 복사본이다 .
- 지속성 계층의 성능을 높이려면 인덱스를 만들어야 한다.

- [[JPA(Java Persistence API)]]를 사용하면 @Index를 사용하여 코드에서 인덱스를 정의하여 이를 달성할 수 있다.
- 이 [[어노테이션(Annotation)]]은 [[스키마(Schema)]] 생성 프로세스에 의해 해석되어 자동으로 아티팩트를 생성한다.
- [[엔티티(Entity)]]에 대한 인덱스를 지정할 필요는 없다.



## @Index의 정의

- JPA 2.1 사양에 최종적으로 추가되었다.
- 이 [[어노테이션(Annotation)]]을 사용하면 [[테이블(Table)]]에 대한 인덱스를 정의하고 그에 따라 사용자 정의할 수 있다.

```java
@Target({})
@Retention(RUNTIME)
public @interface Index {
    String name() default "";
    String columnList();
    boolean unique() default false;
}
```

- 위에서 확인할 수 있듯이 columnList 속성만 필수이며 정의해야 한다.
- 여기서 주목해야 할 한 가지 측면은 어노테이션이 기본 인덱싱 알고리즘인 btree 변경을 지원하지 않는다는 것이다.

## 예시

- 예시로 학생 [[엔티티(Entity)]]를 구현해 본다.

```java
@Entity
@Table
public class Student implements Serializable {
    @Id
    @GeneratedValue
    private Long id;
    private String firstName;
    private String lastName;

    // getters, setters
}
```

- 모델이 있으면 첫 번째 인덱스를 구현해 본다. 
- @Index [[어노테이션(Annotation)]]을 추가하기만 하면 된다 
- [[@Table]] [[어노테이션(Annotation)]]에서 인덱스의 속성을 열([[Spring/SQL/필드(Field)|필드(Field)]])에 지정할 수 있다.

```java
@Table(indexes = @Index(columnList = "firstName"))
```

- firstName 열을 사용하여 첫 번째 인덱스를 선언했다.
- [[스키마(Schema)]] 생성 프로세스를 실행할 때 유효성을 검사할 수 있다.

```
[main] DEBUG org.hibernate.SQL -
  create index IDX2gdkcjo83j0c2svhvceabnnoh on Student (firstName)
```

- 이제 추가 기능을 표시하는 선언을 수정할 차례이다.
- 인덱스에는 이름이 있어야 한다.
- 기본적으로 지정하지 않으면 공급자 생성 값이 지정된다. 
- 사용자 지정 레이블을 원할 때 _name_ 속성을 추가하기만 하면 된다.

```java
@Index(name = "fn_index", columnList = "firstName")
```

- 위의 코드는 사용자 정의 이름으로 인덱스를 생성한다.

```
[main] DEBUG org.hibernate.SQL -
  create index fn_index on Student (firstName)
```

- 또한 [[스키마(Schema)]]의 이름을 지정하여 다른 스키마에서 색인을 생성 할 수 있다.

```java
@Index(name = "schema2.fn_index", columnList = "firstName")
```

- 이제 columnList 구문을 자세히 살펴보자.

```
column ::= index_column [,index_column]*
index_column ::= column_name [ASC | DESC]
```

- 이미 알고 있듯이 인덱스에 포함될 열 이름을 지정할 수 있다. 
- 물론 단일 인덱스에 여러 열을 지정할 수 있다.
- 이름을 쉼표로 구분하여 수행한다.

```java
@Index(name = "mulitIndex1", columnList = "firstName, lastName")
@Index(name = "mulitIndex2", columnList = "lastName, firstName")
```

```
[main] DEBUG org.hibernate.SQL -
  create index mulitIndex1 on Student (firstName, lastName)
   
[main] DEBUG org.hibernate.SQL -
  create index mulitIndex2 on Student (lastName, firstName)
```

지속성 공급자는 지정된 열 순서를 준수해야 합니다. 이 예에서 인덱스는 동일한 열 집합을 지정하더라도 약간 다릅니다.


이전 섹션에서 구문을 검토한 것처럼 _column_name_ 뒤에 _ASC_ (오름차순) 및 _DESC_ (내림차순) 값을 지정할 수도 있습니다  . 인덱싱된 열에 있는 값의 정렬 순서를 설정하는 데 사용합니다.

```java
@Index(name = "mulitSortIndex", columnList = "firstName, lastName DESC")
```

```
[main] DEBUG org.hibernate.SQL -
  create index mulitSortIndex on Student (firstName, lastName desc)
```

각 열의 순서를 지정할 수 있습니다. 그렇지 않으면 오름차순으로 가정합니다.

### 3.4. _@인덱스_ 고유성[](https://recordsoflife.tistory.com/601#4-index-uniqueness)

마지막 선택적 매개변수는 인덱스가 고유한지 여부를 정의 하는 _고유한_ 속성입니다. 고유 인덱스는 인덱싱된 필드가 중복 값을 저장하지 않도록 합니다. 기본적으로 _false_ 입니다. 변경하고 싶다면 다음과 같이 선언할 수 있습니다.

```java
@Index(name = "uniqueIndex", columnList = "firstName", unique = true)
```

```java
[main] DEBUG org.hibernate.SQL -
  alter table Student add constraint uniqueIndex unique (firstName)
```

그런 식으로 인덱스를 만들 때 [_@Column_](https://javaee.github.io/javaee-spec/javadocs/javax/persistence/Column.html) 어노테이션 의 _고유_ 속성 과 마찬가지로 열에 고유성 제약 조건을 추가합니다 . _@Index_ 는 다중 열 고유 제약 조건을 선언할 수 있기 때문에 _@Column_ 보다 이점이 있습니다 .[](https://javaee.github.io/javaee-spec/javadocs/javax/persistence/Column.html)

```java
@Index(name = "uniqueMulitIndex", columnList = "firstName, lastName", unique = true)
```

### 3.5. 단일 엔터티의 여러 _@Index_[](https://recordsoflife.tistory.com/601#5-multiple-index-on-a-single-entity)

지금까지 다양한 색인 변형을 구현했습니다. 물론 엔터티에 대해 단일 인덱스를 선언하는 것으로 제한되지 않습니다. 선언을 수집하고 모든 단일 인덱스를 한 번에 지정합시다. 중괄호로 _@Index_ 어노테이션 을 반복 하고 쉼표로 구분하여 이를 수행합니다.

```
@Entity
@Table(indexes = {
  @Index(columnList = "firstName"),
  @Index(name = "fn_index", columnList = "firstName"),
  @Index(name = "mulitIndex1", columnList = "firstName, lastName"),
  @Index(name = "mulitIndex2", columnList = "lastName, firstName"),
  @Index(name = "mulitSortIndex", columnList = "firstName, lastName DESC"),
  @Index(name = "uniqueIndex", columnList = "firstName", unique = true),
  @Index(name = "uniqueMulitIndex", columnList = "firstName, lastName", unique = true)
})
public class Student implements Serializable
```

또한 동일한 열 집합에 대해 여러 인덱스를 만들 수도 있습니다.

### 3.6. 기본 키[](https://recordsoflife.tistory.com/601#6-primary-key)

인덱스에 대해 이야기할 때 기본 키에서 잠시 멈춰야 합니다. 알다시피 [_EntityManager_](https://recordsoflife.tistory.com/hibernate-entitymanager) 가 관리하는 모든 엔터티 는 기본 키에 매핑되는 식별자를 지정해야 합니다.

일반적으로 기본 키는 특정 유형의 고유 인덱스입니다. 이전에 제시된 방식으로 이 키의 정의를 선언할 필요가 없다는 점을 추가할 가치가 있습니다. 모든 것은 [_@Id_](https://javaee.github.io/javaee-spec/javadocs/javax/persistence/Id.html) 어노테이션에 의해 자동으로 수행됩니다 .

### 3.7. 비 엔터티 _@Index_[](https://recordsoflife.tistory.com/601#7-non-entity-index)

인덱스를 구현하는 다양한 방법을 배운 후에는 _@Table_ 이 인덱스 를 지정하는 유일한 장소가 아니라는 점을 언급 해야 합니다. 같은 방식으로 _@SecondaryTable_ , @CollectionTable, _@JoinTable_ , _@TableGenerator_ 어노테이션 에서 인덱스를 선언할 수 있습니다 . 이러한 예는 이 문서에서 다루지 않습니다. 자세한 내용은 [_javax.persistence_ _JavaDoc을_](https://javaee.github.io/javaee-spec/javadocs/javax/persistence/package-summary.html) 확인하세요 .