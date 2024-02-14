- 데이터 타입을 변환하는 것을 말하며 형변환(Casting)이라고도 한다.
- 자바는 작은 범위(상위)에서 큰 범위(하위)로 당연히 값을 넣을 수 있다.(부모에서 자식으로 다운캐스팅된다.)

- 큰 범위(하위)에서 작은 범위(상위)로 대입은 소수점 버림과 오버플로우 문제가 날 수 있다.(자식에서 부모로 업캐스팅된다.)

- [[List]]와 [[Set]]은 공통적으로 [[Iterator()]]을 사용할 수 있다. 

## 자동 형변환

- 자바에서 작은 범위에서 큰 범위 대입은 허용한다.
- int < long < double 가능하다.(double이 long보다 크다.)

```java
public class Casting {  
  
    public static void main(String[] args) {  
        int intValue = 10;  
        long longValue;  
        double doubleValue;  
  
        longValue = intValue;  
        System.out.println("longValue = " + longValue); // 10
  
        doubleValue = longValue;  
        System.out.println("doubleValue = " + doubleValue); // 10
  
  
        doubleValue = 20L; // 20을 long에 넣은 것  
        System.out.println("doubleValue2 = " + doubleValue); // 20
  
    }  
}
```

- 다음 코드는 내부적인 동작과정이다.

```java
ntValue = 10;
doubleValue = intValue;
doubleValue = (double)intValue; // 형 맞추기
doubleValue = (double)10; // 변수 값 읽기
doubleValue = 10.0; // 형 변환
```

## [[Collection]]의 형변환

- 그럼 두 [[인터페이스(Interface)]]가 공통적으로 가지고 있는 이 [[Iterator()]]라는 속성을 Collection 인터페이스가 가지고 있을까?

- 정확히는 [[List]], [[Set]] => Collection => [[Iterable]]의 순서로 [[implements]] 하고 있다.

![[Pasted image 20240104211341.png]]

- Collection이 [[Iterable]]이라는 [[인터페이스(Interface)]]를, [[List]], [[Set]]이 Collection을 [[Implements]] 하고 있는 것이다.

## 인터페이스 계층 구조 및 ArrayList

![](https://blog.kakaocdn.net/dn/bgbJkF/btrSCLQz8Qd/e94GtwAHsKuND48KMFhEf1/img.jpg)

- 위의 이미지는 Iterable 하위 인터페이스 계층 구조를 나타낸다.

- [[Collection]] [[인터페이스(Interface)]]와 [[List]], [[Java/Set]], Queue 인터페이스의 계층 구조를 통해 Iterable과 Iterator에 대해서 조금 더 살펴보자

- Iterable은 Collection의 상위 인터페이스이다.
- 위에서 본 것처럼 Iterable 인터페이스에는 iterator() 메서드가 선언되어 있다.

- 때문에 Collection 인터페이스 계층구조에서 List, Set, Queue를 구현하는 [[클래스(Class)]]들은 다 iterator() 메서드를 가지고 있다.

- 결론적으로 Iterable의 역할은 iterator() 메서드를 하위 클래스에서 무조건 구현하게 만들기 위함이라고 볼 수 있다.
- 그 하위 클래스에서는 iterator() 메서드를 구현하여 반환된 Iterator 구현체를 통해 반복 동작을 사용할 수 있게 된다.

- 밑의 코드는 [[ArrayList]]를 통한 구현코드 예시이다.

```java
public class ArrayList<E> extends AbstractList<E>
        implements List<E>, RandomAccess, Cloneable, java.io.Serializable {
 
    ...
     
    public Iterator<E> iterator() { // iterator() 메서드 정의
        return new Itr();
    }

    private class Itr implements Iterator<E> {
        ...

        public boolean hasNext() { // 다음 element가 true or false
            return cursor != size;
        }

        @SuppressWarnings("unchecked")
        public E next() {
            ...
        }
        
        ...
    }  
     ...
}
```

1. [[Iterable]] [[인터페이스(Interface)]]는 [[자식 클래스(sub class)]]가 [[추상 메서드(Abstract Method)]] iterator()를 구현하도록 한다.
2. Iterator [[인터페이스(Interface)]]는 [[자식 클래스(sub class)]]가 [[추상 메서드(Abstract Method)]] hasNext() 및 next()를 구현하도록 한다.
3. [[ArrayList]]는 [[List]]를 구현하고 [[List]]는 [[Collection]]을 [[상속(Inheritance)]]받고 [[Collection]]은 [[Iterable]]을 [[상속(Inheritance)]]받는다.  
      
	- 즉, 다음과 같은 관계를 볼 수 있다.
	- [[Iterable]]  <-  [[Collection]]  <- [[List]]  <-  [[ArrayList]]
	
	- 실질적으로 Iterable, Collection, List는 iterator() 메서드를 선언만 하고 [[ArrayList]]에서 해당 [[메서드(Method)]]가 실제로 구현된다.
      
4. 그리고 위 코드 예시는 ArrayList에서 iterator() 메서드를 구현하는 부분에 대한 자세한 코드이다.

- [[Iterator()]] 메서드는 Iterator를 구현한 Itr [[클래스(Class)]]의 [[객체(Object)]]를 반환한다. (Itr [[클래스(Class)]] 내부에 hasNext(), next() 메서드가 구현된 것도 볼 수 있다.)

## 업캐스팅(upcasting)과 다운캐스팅(downcasting)

- 자바에서 [[상속(Inheritance)]] 관계가 있는 특정 [[객체(Object)]]는 상황에 따라 넓은 범위와 좁은 범위로 해석될 수 있다.

- [[자식 클래스(sub class)]]의 [[객체(Object)]]가 [[부모 클래스(super class)]] 타입으로 형변환 되는 것을 업 캐스팅이라고 한다. 
- [[부모 클래스(super class)]]의 [[객체(Object)]]가 [[자식 클래스(sub class)]] 타입으로 형변환 되는 것을 다운 캐스팅이라고 한다.