- List는 [[인터페이스(Interface)]]이다.

- 저장순서가 유지되고 중복을 허용한다.
- 데이터의 저장공간으로 [[배열(Array)]]을 사용한다.

- 즉, Java의 다형성에 의해서 만약 아래와 같이 list를 List 자료형으로 선언한 경우, 그 구현체를 [[ArrayList]]로도 구현할 수 있지만 [[LinkedList]]로 구현 할수도 있다.

```java
List<Integer> list = new ArrayList<Integer>();
list = new LinkedList<Integer>(); // 요구사항 변경 등의 이유로 구현체가 바뀌어도 호환 가능
```

## [[ArrayList]]

- ArrayList 역시 List [[인터페이스(Interface)]]를 구현하고 있기 때문에 List가 제공하는 기능들을 다 제공할 수 있으므로 List 선언 시 구현 인스턴스의 자료형으로 작성할 수 있다.

- 하지만, List와 다르게 ArrayList의 자료형으로 생성한 경우 LinkedList로 새롭게 구현체를 만들거나 [[형변환(Casting)]]을 할 수는 없다.
List<객체> findAll()

- 이 [[메서드(Method)]]는 모든 Member 객체들을 리스트 형태로 반환한다.

## List의 메서드

### [[add()]]

- 데이터를 추가한다.

```java
List<String> fruits = new ArrayList<>();
fruits.add("apple");
fruits.add("banana");
fruits.add("cherry");

 for (String item : fruits) {
            System.out.println(item);
        }
        
 // 출력 : apple
 // 출력 : banana
 // 출력 : cherry
```


### remove()

- 데이터를 삭제한다.

```java
List<String> fruits = new ArrayList<>();
fruits.add("apple");
fruits.add("banana");
fruits.add("cherry");

fruits.remove("banana");  // return 값은 true or false

for(String item : fruits){
	System.out.println(item);
	}
//출력 : apple
//출력 : cherry
```

### get(int index)

- 지정된 인덱스에 해당하는 데이터를 반환한다.

```java
List<String> fruits = new ArrayList<>();
fruits.add("apple");
fruits.add("banana");
fruits.add("cherry");

fruits.remove("banana");  // return 값은 true or false

for(String item : fruits){
	System.out.println(item);
	}
//출력 : apple
//출력 : cherry
```

### set(int index, E element)

- 지정된 인덱스에 해당하는 데이터를 변경한다.

```java
List<String> fruits = new ArrayList<>();
fruits.add("apple");
fruits.add("banana");
fruits.add("cherry");

fruits.set(1, "orange");

for(String item : fruits){
	System.out.println(item);
    }
    
//출력 : apple
//출력 : orange
//출력 : cherry
```

### size()

- 저장된 데이터의 개수를 반환한다.

```java
List<String> fruits = new ArrayList<>();
fruits.add("apple");
fruits.add("banana");
fruits.add("cherry");

int numOfFruits = fruits.size();

System.out.println(numOfFruits);

//출력 : 3
```

### contains(Object o)

- 지정된 데이터가 포함되어 있는지 확인한다.

```java
List<String> fruits = new ArrayList<>();
fruits.add("apple");
fruits.add("banana");
fruits.add("cherry");

boolean hasBanana = fruits.contains("banana");

System.out.println(hasBanana);

//출력 : true
```


## [[List.of()]]

- List.of()는 JAVA 9에서 도입된 메소드이다.
- Arrays.of()와는 대조적으로, 이 메소드는 변경불가한(unmodifiable) list 객체를 생성한다.

```java
@DisplayName("List.of() 사용방법")
@Test
void usageOfListOf() {
    Integer[] array = new Integer[]{1, 2, 3};
    List<Integer> list = List.of(array);
    assertThat(list).containsExactly(1, 2, 3)
}
```