- [[엔티티(Entity)]]-관계 모델. 테이블간의 관계를 설명해주는 다이어그램이라고 볼 수 있으며, 이를 통해 프로젝트에서 사용되는 DB의 구조를 한눈에 파악할 수 있다.  
- 즉, [[API(Application Programming Interface)]]를 효율적으로 뽑아내기 위한 모델 구조도라고 생각하면 된다.

- 직사각형, 다이아몬드, 타원형 및 연결선과 같은 정의된 기호 집합을 사용하여 [[엔티티(Entity)]], [[연관 관계(Relationships)]] 및 해당 속성의 상호 연결성을 나타낸다. 

- [[엔티티(Entity)]]를 명사로, [[연관 관계(Relationships)]]를 동사로 사용하여 문법 구조를 반영한다.


## ERD 사용법

### 데이터 베이스 모델링

- 관계형 DB에서 주로 널리 사용된다.
- 엔티티와 속성들을 테이블과 컬럼들로 변환할 수 있다.
- 테이블과 관계들을 시각화 할 수 있기 때문에 설계 문제점을 파악할 수 있다.

### 소프트웨어 엔지니어링

- 소프트웨어 기획 단계에서 사용된다.
- 서로 다른 시스템 요소와 서로 간의 관계를 식별하는데 도움된다.

### ERD Notation

![](https://velog.velcdn.com/images%2Fkjhxxxx%2Fpost%2Fa89bb89a-02b6-4bc5-81dd-2946eb575fc8%2Fdownload.jpeg)

- 기본 요소는 Entity, Attribute, Relationship 등이 있다.  
- 확장하여 Weak Entity, Multivalued Attribute, Weak Relationship 이 있다.

#### Entity

- 어떤 시스템인지에 따라 Entity는 사람, 장소, 사건(이벤트), 오브젝트가 될 수도 있다.

#### Weak Entity

- 존재하는 다른 Entity에 의존적인 Entity를 Weak Entity라고 한다.
- 그 자식의 속성들에 의해 식별할 수 없는 Entity이다.  

![](https://velog.velcdn.com/images%2Fkjhxxxx%2Fpost%2Fd36c53c5-3bcd-4aa5-bcb8-970ee419c290%2Fweakentity.jpeg)

#### Attribute

- [[속성(Attribute)]] 는 특성, Entity의 성격, 관계, 또 다른 속성이다.  

![](https://velog.velcdn.com/images%2Fkjhxxxx%2Fpost%2F98f4623c-3e7f-4a19-894e-24a4b7251ea1%2Fattribute.jpeg)

#### Multivalued Attribute

- 한 값 이상의 값을 가진 [[속성(Attribute)]]이다.

![](https://velog.velcdn.com/images%2Fkjhxxxx%2Fpost%2F09ee95b1-d18d-4c27-a76a-3478f0cd8086%2Fmultivalued%20attribute.jpeg)

#### Derived Attribute

- 다른 속성에 기초한 속성이다.
- ERD 에서는 보기 드물다.

![](https://velog.velcdn.com/images%2Fkjhxxxx%2Fpost%2F3593ee1e-1d61-4af9-a6a2-016a32b71d01%2Fderived%20attribute.jpeg)

#### Relationship

- Relationship은 Entity간의 상호작용을 표현한다.

![](https://velog.velcdn.com/images%2Fkjhxxxx%2Fpost%2F1ab0686d-7550-4847-9f0e-41ba71b26841%2Frelationship.jpeg)

#### Cadinality and Ordinality

- Entity들 간의 관계에 대한 추가 정보이다.
- One to many, many to many 관계를 나타낼 수 있다.

![](https://velog.velcdn.com/images%2Fkjhxxxx%2Fpost%2F71289544-f6fe-4c43-b052-647ec830eaf8%2Fcadinality%20and%20ordinality.jpeg)

- 여러 기호들로 관계를 표현할 수 있으나, 기호들만 숙지하여도 충분히 표현이 가능하다.

![](https://velog.velcdn.com/images%2Fkjhxxxx%2Fpost%2F47e06548-068c-46cd-9f4c-9e7f86affc1f%2FERD-Notation.png)

##### 1. One

- 일대일 혹은 일대다 관계이다. 
- 주로 하나의 [[외래 키(Foreign Key)]]가 걸린 관계라도 보면 된다.
##### 2. Many

- [[다대다(ManyToMany)]] 관계이다. 
- 중계 테이블을 통하여 여러개의 데이터를 바라보고 있을 때 사용한다.
##### 3. One (and only one)

- 위의 조건과 동일하게 일대일 관계이나, 하나의 row 끼리만 연결된 데이터이다.
##### 4. Zero or one

- 일대일 혹은 일대다 관계를 가지고 있으나, 필수 조건이 아님을 의미한다.
##### 5. One or many

- 일대일 혹은 [[다대다(ManyToMany)]] 관계를 가지고 있음을 의미한다.
- 관계를 가지고 있으나, 참조되는 row 값들이 불명확함을 의미한다.

##### 6. Zero or many

- 참조하는 [[테이블(Table)]]과의 관계가 불명확한 경우이다.
- 장바구니처럼 row 생성값이 없을수도, 하나일수도, 여러개일 수도 있는 경우이다.

## ERD 작성 방법

#### 1. ERD의 표기법

#### 1-1 개체(Entity) - 사각형으로 표시

- 사물 또는 사건으로 정의
- 사각형으로 표기
- 사각형 안에는 개체명을 기입하며, 단수형으로 명명
- 가능한 대문자로 개체명을 사용
- 유일한 단어로 사용

#### 1-2 속성(Attribute) - 동그라미로 표시

- 개체가 가지고 있는 요소 또는 성질
- 속성은 선으로 연결된 동그라미로 표기
- 속성명은 단수형으로 명명
- 속성명은 개체명과 동일한 명칭으로 사용하지 않음
- 속성 값이 null 인지 고려

#### 1-3 관계(Relationship) - 마름모형으로 표시

- 관계 표시는 대부분 마름모형으로 표기하지만 표기법에 따라 다양하게 표현 가능
- 1:1, 1:n, n:m 등을 파악해야함

이 표기법들을 사용하여  
1. 모든 엔티티들을 정의

- 엔티티는 특정 다이어그램에서 한 번만 나타나야한다
- 모든 엔티티에 대해 사각형을 만들고 이름을 적절하게 지정한다

2. 엔티티 간 관계들을 정의

- 선을 사용하여 연결하고 관계를 설명하는 중간에 다이아몬드를 추가

3. 속성들을 추가

- 쉽게 이해할 수 있도록 의미있는 속성이름을 지정

이러한 방법으로 ERD를 작성하면 된다.