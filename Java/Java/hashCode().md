- int hashCode()로 정의된 hashCode [[메서드(Method)]]는 실행 중에(Runtime) [[객체(Object)]]의 유일한 integer값을 반환한다.

- Object [[클래스(Class)]]에서는 heap에 저장된 객체의 메모리 주소를 반환하도록 되어있다. \(항상 그런 것은 아니다.)

- hashCode는 HashTable과 같은 자료구조를 사용할 때 데이터가 저장되는 위치를 결정하기 위해 사용된다.

## hashCode 정의

```java
public native int hashCode();
```

- 여기서 native 키워드는 메서드가 JNI(Java Native Interface)라는 native code를 이용해 구현되었음을 의미한다. 
- native는 메서드에만 적용가능한 제어자로, C or C++ 등 Java가 아닌 언어로 구현된 부분을 JNI를 통해 Java에서 이용하고자 할 때 사용된다. 우리같은 일반 개발자는 어디에서도 사용할 수 없다.

