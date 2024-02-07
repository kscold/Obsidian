- Collection을 Wrapping하면서, 그 외 다른 [[멤버 변수]]가 없는 상태를 일급 컬렉션이라 한다.
- 즉, [[Collection]]을 포함한 [[클래스(Class)]] 는 반드시 다른 [[멤버 변수]]가 없어야 한다.  

- 불변성 유지를 위하여 사용한다.
- [[final]] 키워드로 일급 컬렉션을 만들 수 없기 때문에 [[Collection]]을 따로 정의해서 사용한다.

## 예시

```java
Map<String, String> map = new HashMap<>();

map.put("1", "A");
map.put("2", "B");
map.put("3", "C");
```

- 간단하게 설명하면 위의 코드를 아래와 같이 Wrapping 하는 것을 이야기한다.

```java
public class GameRanking {

    private Map<String, String> ranks;
    
    public GameRanking(Map<String, String> ranks) {
        this.ranks = ranks;
    }
}
```


## Wrapping의 이점

- Wrapping 함으로써 다음과 같은 이점을 가지게 된다.
	1. 비지니스에 종속적인 자료구조
	2. Collection의 불변성을 보장
	3. 상태와 행위를 한 곳에서 관리
	4. 이름이 있는 컬렉션

### 1. 비지니스에 종속적인 자료구조

- 예를 들어 다음과 같은 조건으로 로또 복권 게임을 만든다고 가정한다.

- 로또 복권은 다음의 조건이 있다.

- 6개의 번호가 존재한다.(보너스 번호는 이번 예제에서 제외한다.)
- 6개의 번호는 서로 중복되지 않아야 한다.

- 일반적으로 이런 일은 서비스 메서드에서 진행한다.

![structure1](https://t1.daumcdn.net/cfile/tistory/99CB784E5CD0EA8B30)

- 서비스 메서드에서 비지니스 로직을 처리한다.
- 로또 번호가 필요한 모든 장소에선 검증로직이 들어가야만 한다.

- `List<Long>` 으로 된 데이터는 모두 검증 로직이 필요할까?
- 신규 입사자분들은 어떻게 이 검증로직이 필요한지 알 수 있까?
- 등등 모든 코드와 도메인을 알고 있지 않다면 언제든 문제가 발생할 여지가 있다.

- 그렇다면 이 문제를 어떻게 깔끔하게 해결할 수 있을까

- 6개의 숫자로만 이루어져야만 하고 6개의 숫자는 서로 중복되지 않아야만 하는 자료구조가 필요하기 때문에 직접 만들면 된다.

- 아래와 같이 해당 조건으로만 생성 할 수 있는 자료구조를 만들면 위에서 언급한 문제들이 모두 해결된다.
- 그리고 이런 클래스를 우린 일급 컬렉션이라고 부른다.

![structure2](https://t1.daumcdn.net/cfile/tistory/99B2793E5CD0EA8D36)

- 이제 로또 번호가 필요한 모든 로직은 이 일급 컬렉션만 있으면 된다.

![structure3](https://t1.daumcdn.net/cfile/tistory/9976E04C5CD0EA8B2E)

- 비지니스에 종속적인 자료구조가 만들어져, 이후 발생할 문제가 최소화 되었다.

### 2. 불변성 유지

- 일급 컬렉션은 컬렉션의 불변을 보장한다.
- 여기서 [[final]]을 사용하면 안되는 이유는 자바의 [[final]]은 정확히는 불변을 만들어주는 것은 아니며, 재할당만 금지하기 때문이다.

```java
    @Test
    public void final도_값변경이_가능하다() {
        //given
        final Map<String, Boolean> collection = new HashMap<>();
        
        //when
        collection.put("1", true);
        collection.put("2", true);
        collection.put("3", true);
        collection.put("4", true);
        
        //then
        assertThat(collection.size()).isEqualTo(4);
    }
```

![immutable1](https://t1.daumcdn.net/cfile/tistory/99F1823C5CD0EA8B3B)

- 위를 실행해보면 값이 추가되는 것을 확인할 수 있다.
- 이미 [[Collection]]은 비어있는 [[HashMap]]으로 선언되었음에도 값이 변경될수 있다는 것이다.

```java
    @Test
    public void final은_재할당이_불가능하다() {
        //given
        final Map<String, Boolean> collection = new HashMap<>();
        
        //when
        collection = new HashMap<>();
        
        //then
        assertThat(collection.size()).isEqualTo(4);
    }
```

- 이 코드는 바로 컴파일에러가 발생한다.

![immutable2](https://t1.daumcdn.net/cfile/tistory/99BF844D5CD0EA8B34)

- final로 할당된 코드에 재할당할순 없기 때문이다.
- 따라서 Java의 final은 재할당만 금지한다.  

- 이외에도 `member.setAge(10)` 과 같은 코드 역시 작동해버리니 반쪽짜리 기능이다.

- 요즘과 같이 소프트웨어 규모가 커지고 있는 상황에서 불변 객체는 아주 중요하다.
- 각각의 객체들이 절대 값이 바뀔일이 없다는게 보장되면 그만큼 코드를 이해하고 수정하는데 사이드 이펙트가 최소화되기 때문이다.

- 자바에서는 final로 그 문제를 해결할 수 없기 때문에 일급 컬렉션(Frist Class Collection)과 래퍼 클래스(Wrapper Class) 등의 방법으로 해결해야하만 한다.

- 그래서 아래와 같이 컬렉션의 값을 변경할 수 있는 메소드가 없는 컬렉션을 만들면 불변 컬렉션이 된다.

![immutable3](https://t1.daumcdn.net/cfile/tistory/99D80B4C5CD0EA8B1C)

- 이 [[클래스(Class)]]는 [[생성자(constructor)]]와 getAmountSum() 외에 다른 [[메서드(Method)]]가 없다.
- 즉, 이 클래스의 사용법은 새로 만들거나 값을 가져오는 것뿐인 것이다.  
- List라는 컬렉션에 접근할 수 있는 방법이 없기 때문에 값을 변경/추가가 안된다.

- 이렇게 일급 컬렉션을 사용하면, 불변 컬렉션을 만들수 있다.

### 3. 상태와 행위를 한곳에서 관리

- 일급 컬렉션의 세번째 장점은 값과 로직이 함께 존재한다는 것이다.

- 이 부분은 [[열거(Enum)]]의 장점과도 일맥상통한다.

- 예를 들어 여러 Pay들이 모여있고, 이 중 NaverPay 금액의 합이 필요하다고 가정해보자.
- 일반적으로는 아래와 같이 작성합니다.

![merge1](https://t1.daumcdn.net/cfile/tistory/99AF33435CD0EA8C30)

- List에 데이터를 담고 Service 혹은 Util 클래스에서 필요한 로직 수행한다.

- 위 상황에서는 문제가 있습다.
- 결국 `pays` 라는 컬렉션과 계산 로직은 서로 관계가 있는데, 이를 코드로 표현이 안된다.

- Pay타입의 상태에 따라 지정된 메서드에서만 계산되길 원하는데, 현재 상태로는 강제할 수 있는 수단이 없다.  
- 지금은 Pay타입의 [[List]]라면 사용될 수 있기 때문에 히스토리를 모르는 분들이라면 실수할 여지가 많다.

- 똑같은 기능을 하는 메서드를 중복 생성할 수 있다.
    - 히스토리가 관리 안된 상태에서 신규화면이 추가되어야 할 경우 계산 메서드가 있다는 것을 몰라 다시 만드는 경우가 빈번하다.
    - 만약 기존 화면의 계산 로직이 변경 될 경우, 신규 인력은 2개의 메서드의 로직을 다 변경해야하는지, 해당 화면만 변경해야하는지 알 수 없다.
    - 관리 포인트가 증가할 확률이 매우 높다.

- 계산 메서드를 누락할 수 있다.
    - 리턴 받고자 하는 것이 Long 타입의 값이기 때문에 꼭 이 계산식을 써야한다고 강제할 수 없다.

- 결국에 네이버페이 총 금액을 뽑을려면 이렇게 해야한다는 계산식을 컬렉션과 함께 두어야 한다.  
- 만약 네이버페이 외에 카카오 페이의 총금액도 필요하다면 더더욱 코드가 흩어질 확률이 높다.

- 그래서 이 문제 역시 일급 컬렉션으로 해결한다.

```java
public class PayGroups {
    private List<Pay> pays;
    
    public PayGroups(List<Pay> pays) {
        this.pays = pays;
    }
    
    public Long getNaverPaySum() {
        return pays.stream()
                .filter(pay -> PayType.isNaverPay(pay.getPayType()))
                .mapToLong(Pay::getAmount)
                .sum();
    }
}
```

- 만약 다른 결제 수단들의 합이 필요하다면 아래와 같이 [[람다(lambda)]]식으로 리팩토링 가능하다.

```java
public class PayGroups {
    private List<Pay> pays;
    
    public PayGroups(List<Pay> pays) {
        this.pays = pays;
    }
    
    public Long getNaverPaySum() {
        return getFilteredPays(pay -> PayType.isNaverPay(pay.getPayType()));
    }
    
    public Long getKakaoPaySum() {
        return getFilteredPays(pay -> PayType.isKakaoPay(pay.getPayType()));
    }
    
    private Long getFilteredPays(Predicate<Pay> predicate) {
        return pays.stream()
                .filter(predicate)
                .mapToLong(Pay::getAmount)
                .sum();
    }
}
```

- 이렇게 PayGroups라는 일급 컬렉션이 생김으로 상태와 로직이 한곳에서 관리된다.

![merge1](https://t1.daumcdn.net/cfile/tistory/9909A6495CD0EA8B35)

### 4. 이름이 있는 컬렉션

- 마지막 장점은 컬렉션에 이름을 붙일 수 있다는 것이다.

- 같은 Pay들의 모임이지만 네이버페이의 List와 카카오페이의 List는 다르다.

- 그렇다면 이 둘을 구분할려면 어떻게 해야할까?
- 가장 흔한 방법은 [[변수(Variable)]]명을 다르게 하는 것이다.

![named1](https://t1.daumcdn.net/cfile/tistory/993B324B5CD0EA8B31)

- 위 코드의 단점은 뭘까?

- 검색이 어렵다.
    - 네이버페이 그룹이 어떻게 사용되는지 검색 시 변수명으로만 검색할 수 있다.
    - 위 상황에서 검색은 거의 불가능하다.
    - 네이버페이의 그룹이라는 뜻은 개발자마다 다르게 지을 수 있다.
    - 네이버페이 그룹은 어떤 검색어로 검색이 가능할까
    
- 명확한 표현이 불가능하다.
    - 변수명에 불과하기 때문에 의미를 부여하기가 어렵다.
    - 이는 개발팀/운영팀간에 의사소통시 보편적인 언어로 사용하기가 어려움을 이야기한다.
    - 중요한 값임에도 이를 표현할 명확한 단어가 없는것이다.

- 위 문제 역시 일급 컬렉션으로 쉽게 해결할 수 있다.
- 네이버페이 그룹과 카카오페이 그룹 각각의 일급 컬렉션을 만들면 이 컬렉션 기반으로 용어사용과 검색을 하면 된다.

![named2](https://t1.daumcdn.net/cfile/tistory/99BAF7335CD0EA8B34)

- 개발팀/운영팀 내에서 사용될 표현은 이제 이 컬렉션에 맞추면 된다. 
- 검색 역시 이 컬렉션 클래스를 검색하면 모든 사용 코드를 찾아낼 수 있다.