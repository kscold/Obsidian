- 데이터 타입을 변환하는 것을 말하며 형변환이라고도 한다.
- [[List]]와 [[Set]]은 공통적으로 [[Iterator]]을 사용할 수 있다. 

그럼 두 [[인터페이스(Interface)]]가 공통적으로 가지고 있는 이 [[Iterator]]라는 속성을 Collection 인터페이스가 가지고 있을까?

- 정확히는 [[List]], [[Set]] => Collection => Iterable의 순서로 [[implements]] 하고 있다.

![[Pasted image 20240104211341.png]]


Collection이 Iterable이라는 인터페이스를, List, Set이 Collection을 Implements 하고 있는 것이다.


## Iterable 인터페이스
- 아래와 같은 [[메서드(Method)]]가 정의되어 있다.
- 마찬가지로 Collection, List, Set 에서도 그냥 이 메서드가 정의만 되어 있다.

```java
/**
* 
@return an Iterator.
*/
Iterator<T> iterator();
```

- 즉, 실제 iterator() 메서드의 구현은 하위 클래스 ([[ArrayList]], LinkedList, etc)에서 구현을 하고 있다.

- Iterable 인터페이스에는 Iterator 구현체를 반환하는 iterator() 메서드가 선언되어 있다.
- 따라서 해당 인터페이스를 상속받은 하위 클래스들은 iterator() 메서드를 구현해야 하고, 해당 메서드의 구현을 통해 iterator를 사용할 수 있다.

- 또한 이 인터페이스는 for-each loop를 사용할 수 있는 [[클래스(Class)]]라는 것을 명시해주는 기능도 제공한다.
- Iterable을 상속받는 클래스는 Iterator를 사용하여 for-each loop를 사용할 수 있게 된다.

```java
//일반적인 for문
for (int i=0; i<length; i++) {
    …
}

//for each문
for (String str : list) {
    System.out.println(str);
}

//for each문이 실제 동작되는 코드 (iterator가 사용됨)
Iterator<String> iterator = list.iterator();
while(iterator.hasNext()) {
    System.out.println(iterator.next());
}
```

- 향상된 for문(The enhanced for statement)이라고 불리는 for-each loop는 일반 for loop처럼 for 키워드로 시작한다.

- 루프 카운터 변수를 선언하고 초기화하는 대신, [[배열(Array)]]의 기본 유형과 동일한 유형인 변수를 선언하고 그 뒤에 콜론과 루프를 돌릴 배열을 차례로 선언한다.
- 루프 본문에서는 인덱스 배열 요소를 사용하는 대신 생성된 루프 변수를 사용할 수 있게 된다.

(배열 또는 컬렉션 클래스를 반복하는데만 사용할 수 있다.)

* java 1.8부터는 Iterable에 forEach default 메서드를 제공하고 있다.

## 인터페이스 계층 구조 및 ArrayList 예시

![](https://blog.kakaocdn.net/dn/bgbJkF/btrSCLQz8Qd/e94GtwAHsKuND48KMFhEf1/img.jpg)

Iterable 하위 인터페이스 계층 구조

- Collection 인터페이스와 [[List]], [[Set]], Queue 인터페이스의 계층 구조를 통해 Iterable과 Iterator에 대해서 조금 더 살펴보자

- Iterable은 Collection의 상위 인터페이스이다.
- 위에서 본 것처럼 Iterable 인터페이스에는 iterator() 메서드가 선언되어 있다.

- 때문에 Collection 인터페이스 계층구조에서 List, Set, Queue를 구현하는 클래스들은 다 iterator() 메서드를 가지고 있다.

- 결론적으로 Iterable의 역할은 iterator() 메서드를 하위 클래스에서 무조건 구현하게 만들기 위함이라고 볼 수 있다.

(그리고 그 하위 클래스에서는 iterator() 메서드를 구현하여 반환된 Iterator 구현체를 통해 반복 동작을 사용할 수 있게 된다.)

```java
public class ArrayList<E> extends AbstractList<E>
        implements List<E>, RandomAccess, Cloneable, java.io.Serializable {
 
    ...
     
    public Iterator<E> iterator() {
        return new Itr();
    }

    private class Itr implements Iterator<E> {
        ...

        public boolean hasNext() {
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

_(ArrayList를 통한 예시)_

1. Iterable 인터페이스는 하위 클래스가 [[추상 메서드(Abstract Method)]] 'iterator()'를 구현하도록 한다.
2. Iterator 인터페이스는 하위 클래스가 추상 메서드 'hasNext()' 및 'next()'를 구현하도록 한다.
3. ArrayList는 List를 구현하고 List는 Collection을 상속받고 Collection은 Iterable을 상속받는다.  
      
	즉, 다음과 같은 관계를 볼 수 있다.
	Iterable  <-  Collection  <- List  <-  ArrayList
	
	실질적으로 Iterable, Collection, List는 **'iterator()'** 메서드를 선언만 하고 ArrayList에서 해당 메서드가 실제로 구현된다.
      
4. 그리고 위 코드 예시는 ArrayList에서 iterator() 메서드를 구현하는 부분에 대한 자세한 코드이다.
- 'iterator()'메서드는 Iterator를 구현한 'Itr' 클래스의 객체를 반환한다. (Itr 클래스 내부에 hasNext(), next() 메서드가 구현된 것도 볼 수 있습니다.)

## 업캐스팅(upcasting) 다운캐스팅(downcasting)

- 자바에서 상속관게가 있는 특정 [[객체(Object)]]는 상황에 따라 넓은 범위와 좁은 범위로 해석될 수 있다.
- 넓은 범위에서 좁은 범위로 해석하는 것을 다운 캐스팅이고 한다.
- 좁은 범위에서 넓은 범위로 해석하는 것을 업 캐스팅이라고 한다.

