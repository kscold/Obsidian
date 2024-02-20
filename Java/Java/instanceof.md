- instanceof는 [[객체(Object)]] 타입을 확인하는 연산자이다.
- 형변환 가능 여부를 확인하며 true / false로 결과를 반환한다.

- 주로 [[상속(Inheritance)]] 관계에서 부모 [[객체(Object)]]인지 자식 [[객체(Object)]]인지 확인하는 데 사용된다.
- 쉽게 말해 instancof는 해당 [[클래스(Class)]]가 자기집([[인터페이스(Interface)]] 혹은 [[클래스(Class)]]의 [[인스턴스(Instance)]]인지)이 맞는지 확인해 주는 것이라고 생각하면 된다.
- instanceof의 오른쪽 값이 부모 객체 왼쪽 값이 자식 객체라고 생각하면 편하다.

- instanceof의 기본 사용방법은 객체 instanceof 클래스를 선언함으로써 사용한다.


## 예시


```java
class Parent{}
class Child extends Parent{}

public class InstanceofTest {

    public static void main(String[] args){

        Parent parent = new Parent();
        Child child = new Child();

        System.out.println( parent instanceof Parent );  // true
        System.out.println( child instanceof Parent );   // true
        System.out.println( parent instanceof Child );   // false
        System.out.println( child instanceof Child );   // true
    }

}
```

- 위의 예시에서왜 세번째는 false가 반환되었을까?

1. parent instanceof Parent : 부모가 본인 집을 찾았으니 true이다.
2. child instanceof Parent : 자식이 상속받은 부모 집을 찾았으니 true (상속을 받았으니 자기 집이라 해도 무방하다.)이다.
3. parent instanceof Child : 부모가 자식 집을 찾았으니 false(자식 집은 자식 집이지 부모 집은 아니니까)이다.
4. child instanceof Child : 자식이 본인 집을 찾았으니 true이다.

- 형 변환이 불가능한 즉 타입이 상위 클래스도 하위 클래스도 아닐 경우에는 에러가 난다.