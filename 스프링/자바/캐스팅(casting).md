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

- 즉, 실제 iterator() 메소드의 구현은 하위 클래스 (ArrayList, LinkedList, etc)에서 구현을 하고 있다.

- ArrayList를 보자.

아래 코드처럼 ArrayList에서 Iterator() 메소드를 구현하고 있다. (return new Itr();)

  

- 내부에서 private Class Itr로 Iterator 인터페이스를 구현하고 있다. (hasNext, next 메소드 구현 + 필요한 메소드 구현)

  

|   |   |   |
|---|---|---|
|1<br><br>2<br><br>3<br><br>4<br><br>5<br><br>6<br><br>7<br><br>8<br><br>9<br><br>10<br><br>11<br><br>12<br><br>13<br><br>14<br><br>15<br><br>16<br><br>17<br><br>18<br><br>19<br><br>20<br><br>21<br><br>22<br><br>23<br><br>24<br><br>25<br><br>26<br><br>27<br><br>28<br><br>29<br><br>30<br><br>31<br><br>32<br><br>33<br><br>34<br><br>35<br><br>36<br><br>37<br><br>38<br><br>39<br><br>40<br><br>41<br><br>42<br><br>43<br><br>44<br><br>45<br><br>46<br><br>47<br><br>48<br><br>49<br><br>50<br><br>51<br><br>52<br><br>53<br><br>54<br><br>55<br><br>56<br><br>57<br><br>58<br><br>59<br><br>60<br><br>61<br><br>62<br><br>63<br><br>64<br><br>65<br><br>66<br><br>67<br><br>68<br><br>69<br><br>70<br><br>71|public Iterator<E> iterator() {<br><br>    return new Itr();<br><br>}<br><br>/**<br><br> * An optimized version of AbstractList.Itr<br><br> */<br><br>private class Itr implements Iterator<E> {<br><br>    int cursor;       // index of next element to return<br><br>    int lastRet = -1; // index of last element returned; -1 if no such<br><br>    int expectedModCount = modCount;<br><br>    public boolean hasNext() {<br><br>        return cursor != size;<br><br>    }<br><br>    @SuppressWarnings("unchecked")<br><br>    public E next() {<br><br>        checkForComodification();<br><br>        int i = cursor;<br><br>        if (i >= size)<br><br>            throw new NoSuchElementException();<br><br>        Object[] elementData = ArrayList.this.elementData;<br><br>        if (i >= elementData.length)<br><br>            throw new ConcurrentModificationException();<br><br>        cursor = i + 1;<br><br>        return (E) elementData[lastRet = i];<br><br>    }<br><br>    public void remove() {<br><br>        if (lastRet < 0)<br><br>            throw new IllegalStateException();<br><br>        checkForComodification();<br><br>        try {<br><br>            ArrayList.this.remove(lastRet);<br><br>            cursor = lastRet;<br><br>            lastRet = -1;<br><br>            expectedModCount = modCount;<br><br>        } catch (IndexOutOfBoundsException ex) {<br><br>            throw new ConcurrentModificationException();<br><br>        }<br><br>    }<br><br>    @Override<br><br>    @SuppressWarnings("unchecked")<br><br>    public void forEachRemaining(Consumer<? super E> consumer) {<br><br>        Objects.requireNonNull(consumer);<br><br>        final int size = ArrayList.this.size;<br><br>        int i = cursor;<br><br>        if (i >= size) {<br><br>            return;<br><br>        }<br><br>        final Object[] elementData = ArrayList.this.elementData;<br><br>        if (i >= elementData.length) {<br><br>            throw new ConcurrentModificationException();<br><br>        }<br><br>        while (i != size && modCount == expectedModCount) {<br><br>            consumer.accept((E) elementData[i++]);<br><br>        }<br><br>        // update once at end of iteration to reduce heap write traffic<br><br>        cursor = i;<br><br>        lastRet = i - 1;<br><br>        checkForComodification();<br><br>    }<br><br>    final void checkForComodification() {<br><br>        if (modCount != expectedModCount)<br><br>            throw new ConcurrentModificationException();<br><br>    }<br><br>}<br><br>[Colored by Color Scripter](http://colorscripter.com/info#e)|[cs](http://colorscripter.com/info#e)|

  

**정리하자면,**

  

- Collection 인터페이스가 Iterable 인터페이스를 상속하고 있다. 

- Iterable 인터페이스는 내부에 Iterator 인터페이스를 리턴하는 메소드를 정의하고 있다.

- List와 Set을 구현하는 클래스에서는 Iterable의 iterator() 메소드를 구현해야 한다. 

- iterator() 메소드는 Iterator 인터페이스를 리턴한다. Iterator 인터페이스는 hasNext(), next() 메소드를 정의하고 있다 ( 이 부분을 구현하는 클래스를 내부적으로 구현해야 한다 )

- Itr 내부 클래스를 만들고 implements Iterator 하여 실제 hasNext(), next() 메소드와 기타 필요한 메소드를 구현하여 return 한다.

  

- 우리가 실제 사용하는 Iterator it = list.iterator() 는 어떤 List / Set 의 구현 클래스인지(ArrayList, LinkedList, HashSet, LinkedSet) 에 따라 Iterator의 메소드도 달라진다고 볼 수 있겠다. 

  

**Iterable :**

ㄴ public Iterator iterator();  메소드를 구현하게 강제하기 위한 인터페이스.

  

**Iterator :**

ㄴ public boolean hasNext();   /  public E next();  메소드를 구현하게 강제하기 위한 인터페이스

  

  

**Iterator Pattern에 대한 이해가 있으면 좀 더 이해가 쉬울듯 하다.**

  

재밌구만!!

  

추가로 알아보기 : Enumeration 과의 차이점.