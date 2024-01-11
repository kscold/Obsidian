- Java에서 컬렉션(Collection)이란 데이터의 집합, 그룹을 의미한다.
- JCF(Java Collections Framework)는 이러한 데이터, 자료구조인 컬렉션과 이를 구현하는 [[클래스(Class)]]를 정의하는 [[인터페이스(Interface)]]를 제공한다.

- 다음은 Java 컬렌션 프레임워크의 상속구조를 나타낸다.

![[Pasted image 20240106200211.png]]

- collection 인터페이스는 List, Set, Queue로 크게 3가지 상위 인터페이스로 분류할 수 있다.
- 그리고 여기에 Map의 경우 Collection 인터페이스를 상속받고 있지 않지만 Collection으로 분류된다.
## Collection 인터페이스의 특징
| 인터페이스 | 구현클래스 | 특징 |
| ---- | ---- | ---- |
| [[Java/Set]] | HashSet<br><br>TreeSet | 순서를 유지하지 않는 데이터의 집합으로 데이터의 중복을 허용하지 않는다. |
| [[List]] | LinkedList<br><br>Vector<br><br>[[ArrayList]] | 순서가 있는 데이터의 집합으로 데이터의 중복을 허용한다. |
| Queue | LinkedList<br><br>PriorityQueue | List와 유사하다. |
| [[Map]] | Hashtable<br><br>HashMap<br><br>TreeMap | 키(Key), 값(Value)의 쌍으로 이루어진 데이터으 집합으로,<br>순서는 유지되지 않으며 키(Key)의 중복을 허용하지 않으나 값(Value)의 중복은 허용한다. |

### 1. [[Java/Set]]인터페이스

- 순서를 유지하지 않는 데이터의 집합으로 데이터의 중복을 허용하지 않는다.

- HashSet
    - 가장빠른 임의 접근 속도를 가지고 있다.
    - 순서를 예측할 수 없다.
- TreeSet
    - 정렬방법을 지정할 수 있다.
### 2. [[List]] 인터페이스

- 순서가 있는 데이터의 집합으로 데이터의 중복을 허용한다.

- LinkedList
    - 양방향 포인터 구조로 데이터의 삽입, 삭제가 빈번할 경우 데이터의 위치정보만 수정하면 되기에 유용하다.
    - 스택, 큐, 양방향 큐 등을 만들기 위한 용도로 쓰인다.
- Vector
    - 과거에 대용량 처리를 위해 사용했으며, 내부에서 자동으로 동기화처리가 일어나 비교적 성능이 좋지 않고 무거워 잘 쓰이지 않음  
- [[ArrayList]]  
    - 단방향 포인터 구조로 각 데이터에 대한 인덱스를 가지고 있어 조회 기능에 성능이 뛰어나다.
### 3. [[Map]] 인터페이스

- 키(Key), 값(Value)의 쌍으로 이루어진 데이터으 집합으로, 순서는 유지되지 않으며 키(Key)의 중복을 허용하지 않으나 값(Value)의 중복은 허용한다.

- Hashtable
    - HashMap보다는 느리지만 동기화 지원한다.
    - null이 불가하다.
- HashMap 
    - 중복과 순서가 허용되지 않으며 null값이 올 수 있다.
- TreeMap
    - 정렬된 순서대로 키(Key)와 값(Value)을 저장하여 검색이 빠르다.