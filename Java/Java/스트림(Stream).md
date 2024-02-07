- 자바 8에서 추가한 스트림(Stream)은 [[람다(lambda)]]를 활용할 수 있는 기술 중 하나이다.

- 자바 8 이전에는 [[배열(Array)]] 또는 [[컬렉션(Collection)]] [[인스턴스(Instance)]]를 다루는 방법은 for 또는 foreach문을 돌면서 요소 하나씩을 꺼내서 다루는 방법이었다. 

- 간단한 경우라면 상관없지만 로직이 복잡해질수록 코드의 양이 많아져 여러 로직이 섞이게 되고, [[메서드(Method)]]를 나눌 경우 루프를 여러 번 도는 경우가 발생한다.

- 스트림은 '데이터의 흐름’이다. 
- 배열 또는 컬렉션 인스턴스에 함수 여러 개를 조합해서 원하는 결과를 필터링하고 가공된 결과를 얻을 수 있다.
- 또한 람다를 이용해서 코드의 양을 줄이고 간결하게 표현할 수 있다.
- 즉, 배열과 컬렉션을 함수형으로 처리할 수 있다.

- 또 하나의 장점은 간단하게 병렬처리(_multi-threading_)가 가능하다는 점이다. 
- 하나의 작업을 둘 이상의 작업으로 잘게 나눠서 동시에 진행하는 것을 병렬 처리(_parallel processing_)라고 한다.
- 즉 쓰레드를 이용해 많은 요소들을 빠르게 처리할 수 있다.

## 스트림 문법

1. 생성하기 : 스트림 인스턴스를 생성한다.
2. 가공하기 : 필터링(_filtering_) 및 맵핑(_mapping_) 등 원하는 결과를 만들어가는 중간 작업(_intermediate operations_)이다.
3. 결과 만들기 : 최종적으로 결과를 만들어내는 작업(_terminal operations_)이다.

```java
전체 -> 맵핑 -> 필터링 1 -> 필터링 2 -> 결과 만들기 -> 결과물
```

### 1. 스트림 만들기(생성하기)

- [[배열(Array)]] 스트림(Arrays.stream())이다.

```java
String[] arr = new String[]{"a", "b", "c"};
Stream<String> stream = Arrays.stream(arr);
```

- [[컬렉션(Collection)]] 스트림(stream())이다.

```java
List<String> list = Arrays.asList("a","b","c");
Stream<String> stream = list.stream();
```

- Stream.builder()이다.

```java
Stream<String> builderStream = Stream.<String>builder()
    .add("a").add("b").add("c")
    .build(); 
```

- [[람다(lambda)]]식 Stream.generate(), iterate()이다.

```java
Stream<String> generatedStream = Stream.generate(()->"a").limit(3);
// 생성할 때 스트림의 크기가 정해져있지 않기(무한하기)때문에 최대 크기를 제한해줘야 한다.

Stream<Integer> iteratedStream = Stream.iterate(0, n->n+2).limit(5); //0,2,4,6,8
```

- 기본 타입형 스트림이다.

```java
IntStream intStream = IntStream.range(1, 5); // [1, 2, 3, 4]
```

- 병렬 스트림(parallelStream())이다.

```java
Stream<String> parallelStream = list.parallelStream();
```

### 2. 중간 연산 (가공하기)

#### Filtering

- 스트림 내 요소들을 하나씩 평가해서 걸러내는 작업, if문 역할한다.

```java
List<String> list = Arrays.asList("a","b","c");
Stream<String> stream = list.stream()
	.filter(list -> list.contains("a"));
    // 'a'가 들어간 요소만 선택  [a]
```

- [[람다(lambda)]]식의 리턴값은 boolean이고 true인 경우만 다음 단계 진행한다.

#### Map 메서드

- 스트림 내 요소들을 하나씩 특정 값으로 변환하는 작업, 값을 변환하기 위한 람다를 인자로 받는다.  
- 스트림을 원하는 모양의 새로운 스트림으로 변환하고싶을 때 사용한다.

```java
Stream<String> stream = list.stream()
	.map(String::toUpperCase);
	//[A,B,C]
    
    .map(Integers::parseInt);
    // 문자열 -> 정수로 변환
```

- 스트림에 있는 값을 원하는 [[메서드(Method)]]에 입력값으로 넣으면 [[메서드(Method)]] 실행 결과(반환 값)가 담긴다.

#### Sorting

- 스트림 내 요소들을 정렬하는 작업, Comparator 사용한다.

```java
Stream<String> stream = list.stream()
	.sorted() // [a,b,c] 오름차순 정렬
    .sorted(Comparator.reverseOrder()) // [c,b,a] (내림차순)
    
List<String> list = Arrays.asList("a","bb","ccc");
Stream<String> stream = list.stream()
	.sorted(Comparator.comparingInt(String::length)) // [ccc,bb,a] //문자열 길이 기준 정렬
```

#### 기타 연산

```java
Stream<String> stream = list.stream()
	.distinct() // 중복 제거
    .limit(max) // 최대 크기 제한
    .skip(n)    // 앞에서부터 n개 skip하기
    .peek(System.out::println) // 중간 작업결과 확인
```

### 3. 최종 연산 (결과 만들기)

#### Calculating

- 기본형 타입을 사용하는 경우 스트림 내 요소들로 최소, 최대, 합, 평균 등을 구하는 연산을 수행할 수 있다.

```java
IntStream stream = list.stream()
	.count()   //스트림 요소 개수 반환
    .sum()     //스트림 요소의 합 반환
    .min()     //스트림의 최소값 반환
    .max()     //스트림의 최대값 반환
    .average() //스트림의 평균값 반환
```

#### Reduction

- 스트림의 요소를 하나씩 줄여가며 누적연산을 수행한다.

```java
IntStream stream = IntStream.range(1,5);
	.reduce(10, (total,num)->total+num);
    // reduce(초기값, (누적 변수,요소)->수행문)
    // 10 + 1+2+3+4+5 = 25
```

#### Collecting

- 스트림의 요소를 원하는 자료형으로 변환한다.

```java
//예시 리스트
List<Person> members = Arrays.asList(new Person("lee",26),
									 new Person("kim", 23),
									 new Person("park", 23));
                    

// toList() - 리스트로 반환
members.stream()
	.map(Person::getLastName)
    .collect(Collectors.toList());
    // [lee, kim, park]
    
// joining() - 작업 결과를 하나의 스트링으로 이어 붙이기
members.stream()
	.map(Person::getLastName)
    .collect(Collectors.joining(delimiter = "+" , prefix = "<", suffix = ">");
    // <lee+kim+park>
    
//groupingBy() - 그룹지어서 Map으로 반환
members.stream()
	.collect(Collectors.groupingBy(Person::getAge));
	// {26 = [Person{lastName="lee",age=26}],
    //  23 = [Person{lastName="kim",age=23},Person{lastName="park",age=23}]}
    
//collectingAndThen() - collecting 이후 추가 작업 수행
members.stream()
	.collect(Collectors.collectingAndThen(Collectors.toSet(),
    									   Collections::unmodifiableSet));
	//Set으로 collect한 후 수정불가한 set으로 변환하는 작업 실행
```

#### Matching

- 특정 조건을 만족하는 요소가 있는지 체크한 결과를 반환한다.
- anyMatch(하나라도 만족하는 요소가 있는지), allMatch(모두 만족하는지), noneMatch(모두 만족하지 않는지) 확인한다.

```java
List<String> members = Arrays.asList("Lee", "Park", "Hwang");

boolean matchResult = members.stream()
						.anyMatch(members->members.contains("w")); 
						//w 를 포함하는 요소가 있는지, True

boolean matchResult = members.stream()
						.allMatch(members->members.length() >= 4); 
						// 모든 요소의 길이가 4 이상인지, False

boolean matchResult = members.stream()
						.noneMatch(members->members.endsWith("t")); 
						// t로 끝나는 요소가 하나도 없는지, True
```

#### Iterating

- forEach로 스트림을 돌면서 실행되는 작업이다.

```java
members.stream()
	.map(Person::getName)
    .forEach(System.out::println);
    //결과를 출력 (peek는 중간, forEach는 최종)
```

#### Finding

- 스트림에서 하나의 요소를 반환한다.

```java
Person person = members.stream()
					.findAny()   // 먼저 찾은 요소 하나 반환, 병렬 스트림의 경우 첫번째 요소가 보장되지 않음
                    .findFirst() // 첫번째 요소 반환
```
