- 일급 컬렉션(First Class Collection)은 **[[Collection]]을 Wrapping하면서 그 외 다른 [[멤버 변수(Member Variable)]]가 없는 상태의 [[클래스(Class)]]**를 말한다.
- 즉, [[Collection]]을 포함한 [[클래스(Class)]]는 반드시 다른 [[멤버 변수(Member Variable)]]가 없어야 한다.
- [[final]] 키워드로는 [[Collection]]의 진정한 불변성을 보장할 수 없기 때문에 일급 컬렉션으로 해결한다.

## Wrapping의 이점

Wrapping을 통해 다음 4가지 이점을 얻는다.

1. 비즈니스에 종속적인 자료구조
2. Collection의 불변성 보장
3. 상태와 행위를 한 곳에서 관리
4. 이름이 있는 컬렉션

## 기본 예시

```java
// Wrapping 전 — 일반 컬렉션
Map<String, String> map = new HashMap<>();
map.put("1", "A");
map.put("2", "B");
map.put("3", "C");

// Wrapping 후 — 일급 컬렉션 (다른 멤버 변수 없음)
public class GameRanking {
    private Map<String, String> ranks;

    public GameRanking(Map<String, String> ranks) {
        this.ranks = ranks;
    }
}
```

## 1. 비즈니스에 종속적인 자료구조

로또 복권 게임을 예시로 든다. 조건: 6개의 번호, 서로 중복되지 않음.

### 일급 컬렉션 적용 전

```java
// 검증 로직이 서비스에 흩어짐 — 로또 번호가 필요한 모든 곳에 중복 작성 필요
public class LottoService {
    private static final int LOTTO_NUMBERS_SIZE = 6;

    public void createLottoNumber() {
        List<Long> lottoNumbers = createNonDuplicateNumbers();
        validateSize(lottoNumbers);
        validateDuplicate(lottoNumbers);
        // 이후 로직 쭉쭉 실행
    }

    private void validateSize(List<Long> lottoNumbers) {
        if (lottoNumbers.size() != LOTTO_NUMBERS_SIZE) {
            throw new IllegalArgumentException("로또 번호는 6개만 가능합니다.");
        }
    }

    private void validateDuplicate(List<Long> lottoNumbers) {
        Set<Long> nonDuplicateNumbers = new HashSet<>(lottoNumbers);
        if (nonDuplicateNumbers.size() != LOTTO_NUMBERS_SIZE) {
            throw new IllegalArgumentException("로또 번호들은 중복될 수 없습니다.");
        }
    }
}
```

- `List<Long>` 어디서든 검증 로직이 반복된다.
- 신규 개발자는 어디서 검증이 필요한지 알 수 없다.

### 일급 컬렉션 적용 후

```java
// 일급 컬렉션 — 6개 / 중복되지 않은 숫자만 가능한 자료구조
public class LottoTicket {
    private static final int LOTTO_NUMBERS_SIZE = 6;

    private final List<Long> lottoNumbers;

    public LottoTicket(List<Long> lottoNumbers) {
        validateSize(lottoNumbers);       // 생성 시점에 검증 강제
        validateDuplicate(lottoNumbers);
        this.lottoNumbers = lottoNumbers;
    }

    private void validateSize(List<Long> lottoNumbers) {
        if (lottoNumbers.size() != LOTTO_NUMBERS_SIZE) {
            throw new IllegalArgumentException("로또 번호는 6개만 가능합니다.");
        }
    }

    private void validateDuplicate(List<Long> lottoNumbers) {
        Set<Long> nonDuplicateNumbers = new HashSet<>(lottoNumbers);
        if (nonDuplicateNumbers.size() != LOTTO_NUMBERS_SIZE) {
            throw new IllegalArgumentException("로또 번호들은 중복될 수 없습니다.");
        }
    }
}

// 서비스는 단순히 LottoTicket을 생성하기만 하면 됨 — 필요한 로직은 모두 LottoTicket으로
public class LottoService {
    public void createLottoNumber() {
        LottoTicket lottoTicket = new LottoTicket(createNonDuplicateNumbers());
        // 이후 로직 쭉쭉 실행
    }
}
```

- 비즈니스에 종속적인 자료구조가 만들어져 이후 발생할 문제가 최소화된다.

## 2. 불변성 유지

[[final]] 키워드는 **재할당만 금지**할 뿐, [[Collection]] 내부 값 변경은 막지 못한다.

```java
@Test
void final도_값변경이_가능하다() {
    // given
    final Map<String, Boolean> collection = new HashMap<>();

    // when — final 선언에도 불구하고 값 추가 가능
    collection.put("1", true);
    collection.put("2", true);
    collection.put("3", true);
    collection.put("4", true);

    // then — 테스트 통과 (값이 추가됨)
    assertThat(collection.size()).isEqualTo(4);
}

@Test
void final은_재할당이_불가능하다() {
    // given
    final Map<String, Boolean> collection = new HashMap<>();

    // when — 컴파일 에러: Cannot assign a value to final variable 'collection'
    // collection = new HashMap<>();  // 이 줄은 컴파일 에러 발생

    assertThat(collection.size()).isEqualTo(0);
}
```

- `final`은 참조 재할당만 금지 — `collection.put()`으로 내부 값 변경은 가능하다.
- `member.setAge(10)` 같은 코드도 `final`로는 막을 수 없다.

### 일급 컬렉션으로 진정한 불변성 구현

```java
public class LottoTicket {
    private final List<Long> lottoNumbers;

    public LottoTicket(List<Long> lottoNumbers) {
        this.lottoNumbers = new ArrayList<>(lottoNumbers);  // 방어적 복사
    }

    // 값을 변경하는 메서드가 없음 — 진정한 불변
    public List<Long> getLottoNumbers() {
        return Collections.unmodifiableList(lottoNumbers);  // 읽기 전용으로 노출
    }
}
```

- 이 [[클래스(Class)]]는 [[생성자(constructor)]]와 게터 외에 값을 변경하는 [[메서드(Method)]]가 없다.
- [[Collection]]에 접근할 수 있는 방법이 없으므로 값을 변경/추가할 수 없다.

## 3. 상태와 행위를 한 곳에서 관리

```java
// 기존 방식 — 상태(pays)와 로직(계산)이 서로 다른 클래스에 흩어짐
public class PayService {
    public Long getNaverPaySum(List<Pay> pays) {
        return pays.stream()
                .filter(pay -> PayType.NAVER_PAY.equals(pay.getPayType()))
                .mapToLong(Pay::getAmount)
                .sum();
        // 다른 서비스에서도 같은 계산 로직이 중복될 수 있음
    }
}
```

```java
// 일급 컬렉션 적용 — 상태(pays)와 행위(계산)를 한 곳에서 관리
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

- `PayGroups`라는 일급 컬렉션이 생김으로 상태와 로직이 한 곳에서 관리된다.
- 계산 로직 중복 방지, 수정 시 한 곳만 변경하면 된다.
- [[열거(Enum)]]의 상수 + 행위 결합과 같은 이점이다.

이 부분은 [[열거(Enum)]]의 장점과도 일맥상통한다.

## 4. 이름이 있는 컬렉션

```java
// 기존 방식 — 변수명으로만 구분, 코드베이스 검색 불가
List<Pay> naverPays = pays.stream()
        .filter(pay -> PayType.NAVER_PAY.equals(pay.getPayType()))
        .collect(Collectors.toList());

List<Pay> kakaoPays = pays.stream()
        .filter(pay -> PayType.KAKAO_PAY.equals(pay.getPayType()))
        .collect(Collectors.toList());

// 일급 컬렉션 — 타입으로 명확히 구분, 코드베이스 전체 검색 가능
NaverPays naverPays = new NaverPays(pays);
KakaoPays kakaoPays = new KakaoPays(pays);
```

- [[변수(Variable)]]명에 불과하면 검색이 불가능하고 개발자마다 다른 이름을 붙인다.
- 일급 컬렉션으로 이름을 부여하면 팀 내 공통 언어(유비쿼터스 언어)로 사용 가능하고 코드 검색도 용이하다.
- 개발팀/운영팀 내에서 사용될 표현은 이제 이 컬렉션 클래스를 기준으로 맞추면 된다.

## 관련

- [[Collection]]
- [[클래스(Class)]]
- [[멤버 변수(Member Variable)]]
- [[final]]
- [[열거(Enum)]]
- [[람다(lambda)]]
- [[객체지향(Object Oriented Programming)]]
- [[캡슐화(encapsulation)]]
- [[생성자(constructor)]]
- [[메서드(Method)]]
