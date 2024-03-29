- [[리파지터리(Repository)]]가 DB와 데이터를 주고 받으려면, 알맞은 규격이 필요하다. 
- 마치 편지를 보내려면 편지 봉투가 필요한 경우와 비슷하고 이를 엔티티(entity)라 한다.
- 즉, 정보를 표현하는 단위이며 다른 말로 domain이라고 부른다.

- 따라서 자바 [[객체(Object)]]를 [[데이터베이스(DataBase)]]가 이해할 수 있게 만든 것으로, 이를 기반으로 [[테이블(Table)]]이 만들어진다.

- 즉, DB 데이터를 담는 자바 [[객체(Object)]]로, 엔티티를 기반으로 [[테이블(Table)]]을 생성한다.
- 엔티티를 설명하는 특징을 [[속성(Attribute)]]([[Spring/SQL/필드(Field)|필드(Field)]])라고 한다.
- 특정 [[클래스(Class)]]가 엔티티임을 선언하기 위해 [[@Entity]] 어노테이션을 붙인다.

- 단, Domain Logic만 가지고 있어야 하고 Presentation Logic을 가지고 있어서는 안된다.
- 여기서 구현한 [[메서드(Method)]]는 주로 [[서비스(Service)]] Layer에서 사용된다.

- 모든 [[JPA(Java Persistence API)]] 엔티티는 hibernate를 사용하다는 기준으로 기본 [[생성자(constructor)]]를 꼭 가지고 있어야하고, 굳이 [[public]]일 필요가 없으므로 protected로 선언한다.
- 따라서 기본 생성자를 위해 [[롬복(lombok)]]의 [[@AllArgsConstructor]]과 [[@NoArgsConstructor]]를 사용한다.
- [[private]]는 오류가 난다.

- [[@OneToMany]] [[어노테이션(Annotation)]]을 달고 있는 엔티티가 부모 엔티티가 된다.


![[Pasted image 20240106023137.png]]

## 예시

- 엔티티 entity는 현실 세계에 존재하는 것을 데이터베이스 상에서 표현하기 위해 사용하는 추상적인 개념이다. 
- 일종의 비유라고 할 수 있다.

![[Pasted image 20240106023715.png]]

- 위의 예시에서 고객을 관리하기 위해 사용하는 데이터베이스 예제에서 ID, 나이, 클래스 라는 정보들을 통해 '고객'이라는 엔티티(객체)를 표현할 수 있고, 동시에 구분할 수 있다.
- 1행의 고객과 2행의 고객을 구분하기 위해 해당 고객의 이름이나 ID를 비교할 수 있다.
- 현실 세계에서 사람들(엔티티)을 구분하기 위해 이름, 주민등록번호, 출신지, 성별 등의 특성을 이용하는 것과 마찬가지이다.

- 하나 이상의 엔티티들의 모임을 엔티티 셋(Entity set)이라고 부른다.

## 엔티티와 [[레코드(Record)]]드의 차이점(Entity vs Records)

- 레코드는 실제 데이터베이스 상에 저장되어 있는 값들의 모임을 말한다.
- 반면에, 엔티티는 현실 세계에 존재하는 [[객체(Object)]]를 표현하기 위해 비유(추상)적으로 사용된다.

##  [[JPA(Java Persistence API)]]에서 엔티티로 [[테이블(Table)]] 선언방법

1. [[DTO(Data Transfer Object)]] 코드를 작성할 때, 같이 [[Spring/SQL/필드(Field)|필드(Field)]]를 선언하고 이 [[Spring/SQL/필드(Field)|필드(Field)]]들을 DB에서 인식할 수 있게 [[@Column]] [[어노테이션(Annotation)]]을 붙인다.
2. 따라서 이 [[Spring/SQL/필드(Field)|필드(Field)]] 들이 DB 테이블의 각 열(column)과 연결된다.
3. 엔티티의 대푯값([[기본 키(Primary Key)]])을 설정하는데 [[기본 키(Primary Key)]]을 id로 선언하고 [[@Id]] [[어노테이션(Annotation)]]을 붙인다.
4. 이이서 [[@GeneratedValue]] 어노테이션도 붙여서 [[기본 키(Primary Key)]]을 자동으로 생성하게 한다.



## Entity 클래스와 [[DTO(Data Transfer Object)]] 클래스를 분리하는 이유  


- View Layer와 DB Layer의 역할을 철저하게 분리하기 위해서     
- 테이블과 매핑되는 Entity 클래스가 변경되면 여러 클래스에 영향을 끼치게 되는 반면 View와 통신하는 DTO 클래스(Request/ Response 클래스) 는 자주 변경되므로 분리해야 한다.  

- Domain Model을 아무리 잘 설계했다고 해도 각 View 내에서 Domain Model의 getter만을 이용해서 원하는  
       원하는 정보를 표시하기가 어려운 경우가 종종 있다.
- 이런 경우 Domian Model내에 Presentation을 위한 필드나 로직을 추가하게 되는데, 이러한 방식이 모델링 순수성을 깨고 Domain Model 객체를 망가뜨리게 된다.  

- 또한 Domain Model을 복잡하게 조합한 형태의 Presentation 요구사항들이 있기 때문에   
       Domain Model을 직접 사용하는 것은 어렵다.  

-  즉, DTO는 Domain Model을 복사한 형태로, 다양한 Presentation Logic을 추가한 정도로 사용하며,   
       Domain Model 객체는 Persistent만을 위해서 사용된다.