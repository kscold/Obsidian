T- [[@Id]]는 해당 [[속성(Attribute)]]([[Spring/SQL/필드(Field)|필드(Field)]])가 [[테이블(Table)]]의 주키([[기본 키(Primary Key)]]) 역할을 한다는 것을 나타낸다.
- @GeneratedValue는 [[기본 키(Primary Key)]]의 값을 위한 자동 생성 전략을 명시하는데 사용한다.

- [[기본 키(Primary Key)]] 생성 전략으로 [[JPA(Java Persistence API)]]가 지원하는 것은 IDENTITY, SEQUENCE, TABLE, AUTO  4가지이다.
- @GeneratedValue(strategy =  GenerationType.전략) 형식으로 지정한다.

## strategy
### 1. AUTO 

- persistence provider가 특정 DB에 맞게 자동 선택한다.
- 데이터베이스 벤더에 의존하지 않고, 데이터베이스는 [[기본 키(Primary Key)]]를 할당하는 벙법이다.
- 데이터베이스에 따라서 IDENTITY, SEQUENCE, TABLE 방법 중 하나를 자동으로 선택해주는 방법이다.  
- 예를들어, Oracle일 경우 SEQUENCE를 자동으로 선택해서 사용합니다. 따라서, 데이터베이스를 변경해도 코드를 수정할 필요가 없다.

### 2. IDENTITY 

- DB의 identity 컬럼을 이용한다.
- [[기본 키(Primary Key)]] 생성을 데이터베이스에 위임하는 방법(데이터베이스에 의존적)이다. 
- 주로 MySQL, PostgresSQL, SQL Server, DB2에서 사용한다.

- 만약 [[테이블(Table)]]을 [[DROP]]하고 다시 key를 1부터 실행시키고 싶으면 아래 [[SQL]] [[쿼리(query)]]를 실행시키면 된다.

```sql
ALTER TABLE your_table_name AUTO_INCREMENT = 1;
```

### 3. SEQUENCE 

- DB의 시퀀스 컬럼을 이용
- 데이터베이스 시퀀스를 사용해서 기본 키를 할당하는 방법(데이터베이스에 의존적)이다.
- 주로 시퀀스를 지원하는 Oracle, PostgresSQL, DB2, H2에서 사용한다.   
- @SequenceGenerator를 사용하여 시퀀스 생성기를 등록하고, 실제 데이터베이스의 생성될 시퀀스이름을 지정해줘야 한다.
  
### 4. TABLE 

- 유일성(unique)이 보장된 데이터베이스 [[테이블(Table)]]을 이용한다.
- 키 생성 [[테이블(Table)]]을 사용하는 방법이다.
- 키 생성 전용 테이블을 하나 만들고 여기에 이름과 값으로 사용할 컬럼을 만드는 방법이다.  
- 테이블을 사용하므로, 데이터베이스 벤더에 상관없이 모든 데이터베이스에 적용이 가능하다.  
  
- postgres를 이용하여 테스트해보면 AUTO와 SEQUENCE는 실제 [[INSERT]] 쿼리가 일어나기 전에 다음 쿼리를 통해서 주키를 가져오는 것을 확인할 수 있다.
- [[SELECT]] nextval ('hibernate_sequence')

  
## 기본키 자동생성 사용방법

- [[@Id]] [[어노테이션(Annotation)]]을 사용한다.
- @GeneratedValue를 사용한다.
- @GeneratedValue에 원하는 키 생성 전략을 선택한다. (IDENTITY, SEQUENCE, TABLE, AUTO)

## 예시

```java
@Entity
public class Team {
  @Id
  @Column(name = "team_id")
  @GeneratedValue(strategy = GenerationType.IDENTITY)
  private Long id;
}


@Entity
@SequeceGenerator(name = "TEAM_SEQ_GENERATOR", sequenceName = "TEAM_SEQ", initialValue = 1, allocationSize = 1)
// name=식별자 생성기 이름, sequenceName=DB에 등록될 시퀀스이름, initialValue=최초시작하는 수, allocationSize=증가하는수)
public class Team {

  @Id
  @Column(name = "team_id")
  @GeneratedValue(strategy = GenerationType. SEQUENCE, generator = "TEAM_SEQ_GENERATOR")
  private Long id;

}

@Entity
@TableGenerator(name="TEAM_SEQ_GENERATOR", table="TEAM_SEQUENCES", pkColumnValue="TEAM_SEQ", allocationSize=1)
// name=식별자 생성기 이름, table=키생성 테이블 이름, pkColumnValue=DB에 등록될 시퀀스이름)
public class Team {
  @Id
  @Column(name = "team_id")
  @GeneratedValue(strategy = GenerationType. TABLE, generator = "TEAM_SEQ_GENERATOR")
  private Long id;

}

@Entity
public class Team {
  @Id
  @Column(name = "team_id")
  @GeneratedValue(strategy = GenerationType.AUTO)
  private Long id;

}
```