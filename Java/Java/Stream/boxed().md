- boxed() [[메서드(Method)]]는 IntStream 같이 [[원시 타입(Primitive Type)]]에 대한 [[스트림(Stream)]] 지원을 클래스 타입([[IntStream]]-> Stream`<Integer>`)으로 전환해준다.

- 전용으로 실행 가능한 기능을 수행하기 위해 존재한다.
- 예를 들어 int 자체로는 [[Collection]]에 못 담기 때문에 Integer [[클래스(Class)]]로 변환하여 [[List]]`<Integer>` 로 담기 위해 t사용한다.

## int[] -> Integer[]

```java
public static void main(String[] args) {
		
        //int[] -> IntStream -> Stream<Integer> -> Integer[]
        int[] num = {3, 4, 5};
		
        //1. int[] -> IntStream
        IntStream stream = Arrays.stream(num); // IntStream으로 변환
		
        //2. IntStream -> Stream<Integer>
        Stream<Integer> boxed = stream.boxed(); // 일반 Stream으로 변환
		
        //3. Stream<Integer> -> Integer[]
        Integer[] result = boxed.toArray(Integer[]::new); 
        // toArray 메서드를 이용하여 Integer로 변환
		
        System.out.println(Arrays.toString(result)); // 문자열로 변환
		
        // one line 위 과정을 한줄로 표현
        Integer[] oneLineResult = Arrays.stream(num)
        								.boxed()
                                        .toArray(Integer[]::new);
    }
```

## int[] -> [[List]]`<Integer>`

```java
List<Integer> list1 = Arrays.stream(arr).boxed().collect(Collectors.toList());
List<Integer> list2 = IntStream.of(arr).boxed().collect(Collectors.toList());
```


## [[List]]`<Integer>` -> int[]

```java
// 방법 1 - 직접 int[] 생성해서 넣기
int[] arr1 = new int[list.size()]
for (int i = 0 ; i < list.size() ; i++) 
      arr1[i] = list.get(i).intValue();
    
// 방법 2
int[] arr2 = list.stream()
                 .mapToInt(i -> i)
                 .toArray();
    
// 방법 3
int[] arr3 = list.stream()
                 .mapToInt(Integer::intValue)
                 .toArray();
```


## int[] -> [[Set]]`<Integer>`

```java
// 방법 1) HashSet의 인자에 List<Integer> 넣기
int[] ints = {1, 2, 3, 4};
Set<Integer> integerHashSet = new HashSet<>(Arrays.stream(ints).boxed().collect(Collectors.toList()));

// 방법 2) stream으로 한 줄에 작성
Set<Integer> set = Arrays.stream(ints).boxed().collect(Collectors.toSet());

// 방법 3) int[] -> Integer[]
HashSet<Integer> hashset = IntStream.of(ints).boxed().collect(Collectors.toCollection(HashSet::new));

// 방법 4) add로 for문 돌려서 넣기
Set <Integer> integerHashSet = new HashSet <Integer>();
for (Integer t: ints){  
   integerHashSet.add(t); 
}       
```