- Java에서 컬렉션(Collection)이란 데이터의 집합, 그룹을 의미한다.
- JCF(Java Collections Framework)는 이러한 데이터, 자료구조인 컬렉션과 이를 구현하는 [[클래스(Class)]]를 정의하는 [[인터페이스(Interface)]]를 제공한다.

## 컬렉션 프레임워크

- 다음은 Java 컬렉션 프레임워크의 상속구조를 나타낸다.

![[Pasted image 20240106200211.png]]

- collection 인터페이스는 [[List]], [[Set]], Queue로 크게 3가지 상위 [[인터페이스(Interface)]]로 분류할 수 있다.
- 그리고 여기에 [[Map]]의 경우 Collection 인터페이스를 상속받고 있지 않지만 Collection으로 분류된다.
## Collection 인터페이스의 특징
| 인터페이스 | 구현클래스 | 특징 |
| ---- | ---- | ---- |
| [[Set]] | [[HashSet]]<br><br>TreeSet | 순서를 유지하지 않는 데이터의 집합으로 데이터의 중복을 허용하지 않는다. |
| [[List]] | [[LinkedList]]<br><br>Vector<br><br>[[ArrayList]] | 순서가 있는 데이터의 집합으로 데이터의 중복을 허용한다. |
| Queue | LinkedList<br><br>PriorityQueue | List와 유사하다. |
| [[Map]] | Hashtable<br><br>[[HashMap]]<br><br>TreeMap | 키(Key), 값(Value)의 쌍으로 이루어진 데이터으 집합으로,<br>순서는 유지되지 않으며 키(Key)의 중복을 허용하지 않으나 값(Value)의 중복은 허용한다. |

### 1. [[Set]] 인터페이스

- 순서를 유지하지 않는 데이터의 집합으로 데이터의 중복을 허용하지 않는다.

- HashSet
    - 가장빠른 임의 접근 속도를 가지고 있다.
    - 순서를 예측할 수 없다.
- TreeSet
    - 정렬방법을 지정할 수 있다.
### 2. [[List]] 인터페이스

- 순서가 있는 데이터의 집합으로 데이터의 중복을 허용한다.

- [[LinkedList]]
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
- [[HashMap]]
    - 중복과 순서가 허용되지 않으며 null값이 올 수 있다.
- TreeMap
    - 정렬된 순서대로 키(Key)와 값(Value)을 저장하여 검색이 빠르다.


## 컬렉션과 스트림의 차이

- 데이터를 **언제** 계산하느냐가 컬렉션과 [[스트림(Stream)]]의 가장 큰 차이이다.

- 컬렉션은 우리가 아는 DVD와 비슷하다.
- 영상 전체 데이터를 CD에 모두 담고 있다. 

- 컬렉션은 현재 자료구조가 포함하는 모든 값을 메모리에 저장하는 자료 구조다.
- 즉, 컬렉션의 모든 요소는 컬렉션에 추가하기 전에 계산되어야 한다. 

- 반면, 스트림은 스트리밍 서비스와 비슷하다.
- 사용자가 필요로 하는 몇 부분만 미리 내려받는다.

- 스트림은 이론적으로 요청할 때만 요소를 계산하는 고정된 자료구조이다.
- 사용자가 요청하는 값만 추출하는 것이 스트림의 핵심이다.

- 또 다른 컬렉션과 스트림의 차이점은 **외부 반복**과 **내부 반복**이다.

- 컬렉션을 사용하려면 사용자가 직접 요소를 반복( 예) [[forEach()]] 사용)해야 하는데 이를 외부 반복이라 한다. 
- 반면 스트림은 반복을 알아서 처리하고 결과 스트림 값을 어딘가에 저장해 주는 내부 반복을 사용한다.

- 예제를 들어 조금 더 자세히 알아보자. 수학점수와 학년을 변수로 갖는 Score 리스트가 있다.

```java
public static final List<Score> scores = Arrays.asList(
	new Score(100, GRADE.GRADE4)
    ,new Score(80, GRADE.GRADE1)
    ,new Score(17, GRADE.GRADE3)
    ,new Score(50, GRADE.GRADE4)
    ,new Score(28, GRADE.GRADE3)
    ,new Score(30, GRADE.GRADE2)
    ,new Score(98, GRADE.GRADE1)
    ,new Score(69, GRADE.GRADE4)
    ,new Score(87, GRADE.GRADE3)
    ,new Score(76, GRADE.GRADE2)
);

// Score 클래스 일부 소스코드
public static class Score{
    private int math;
    private GRADE grade;

    public Score(int math, GRADE grade) {
        this.math = math;
        this.grade = grade;
    }

    @Override
    public String toString() {
        return "Score{" +
                "math=" + math +
                ", grade=" + grade +
                '}';
    }
}
```

- `List<Score>`에서 학년이 4학년인 학생들의 수학 점수를 추출하려고 한다. 
- [[스트림(Stream)]]을 활용한 아래의 코드가 실행되면 출력이 어떻게 될까?

```java
scores.stream()
    .filter(g -> {
        System.out.println("filter 연산 : " + g.toString());
        return g.getGrade().equals(GRADE.GRADE4); // GRADE4와 같은 데이터만 리턴
    })
    .map(m -> {
        System.out.println("map 연산  : " + m.toString());
        return m.getMath();
    })
    .limit(2)
    .forEach(System.out::println);
```

- 정답을 생각해 보면 4학년 학생의 수학점수 100, 50이 출력이 될 것이다. 
- 그리고 중간연산 filter, map의 print문을 한번 살펴보자. 출력 결과는 아래와 같다.

```java
filter 연산 : Score{math=100, grade=GRADE4}
map 연산  : Score{math=100, grade=GRADE4}
100
filter 연산 : Score{math=80, grade=GRADE1}
filter 연산 : Score{math=17, grade=GRADE3}
filter 연산 : Score{math=50, grade=GRADE4}
map 연산  : Score{math=50, grade=GRADE4}
50
```

- `List<Score>`에는 10개의 데이터가 있었지만 모두 실행되지 않았다.
- 출력 내용을 하나씩 살펴보자.

- Score(100, GRADE.GRADE4)인 학생은 4학년에 해당되므로 중간연산 filter, map의 출력문이 실행되고, [[forEach()]]문이 실행되었다.

```java
filter 연산 : Score{math=100, grade=GRADE4}
map 연산  : Score{math=100, grade=GRADE4}
100
```

- Score(80, GRADE.GRADE1)인 학생은 1학년이므로, filter의 출력문만 실행되었다.

```java
filter 연산 : Score{math=80, grade=GRADE1}
```

- Score(17, GRADE.GRADE3)인 학생은 3학년이므로, filter의 출력문만 실행되었다.

```java
filter 연산 : Score{math=17, grade=GRADE3}
```

- Score(50, GRADE.GRADE4)인 학생은 4학년에 해당되므로 중간연산 filter, map의 출력문이 실행되고, [[forEach()]]문이 실행되었다.

```java
filter 연산 : Score{math=50, grade=GRADE4}
map 연산  : Score{math=50, grade=GRADE4}
50
```

- 그 이후의 데이터는 [[limit()]], 매개변수 2를 만족하는 결과를 얻었기 때문에 연산이 수행되지 않았다.