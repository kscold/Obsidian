- [[리파지터리(repository)]]가 DB와 데이터를 주고 받으려면, 알맞은 규격이 필요하다. 
- 마치 편지를 보내려면 편지 봉투가 필요한 경우와 비슷하고 이를 엔티티(entity)라 한다.
- 따라서 자바 [[객체(Object)]]를 DB가 이해할 수 있게 만든 것으로, 이를 기반으로 테이블이 만들어진다.
- 엔티티를 설명하는 특징을 [[속성(Attribute)]]라고 한다.
- 특정 클래스가 엔티티임을 선언하기 위해 [[@Entity]] 어노테이션을 붙인다.


![[Pasted image 20240106023137.png]]

## 좀 더 쉬운 엔티티의 설명

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

## 엔티티로 테이블 선언방법

1. [[DTO(Data Transfer Object)]] 코드를 작성할 때, 같이 필드를 선언하고 이 필드들을 DB에서 인식할 수 있게 [[@Column]] [[어노테이션(Annotation)]]을 붙인다.
2. 따라서 이 필드 들이 DB 테이블의 각 열(column)과 연결된다.
3. 엔티티의 대푯값을 넣는다. 대푯값을 id로 선언하고 [[@Id]] 어노테이션을 붙인다.
4. 이이서 [[@GeneratedValue]] 어노테이션도 붙여서 대푯값을 자동으로 생성하게 한다.