- 자바 문자열의 [[객체(Object)]]의 내용 혹은 문자열([[String]])비교를 할 때 사용한다.

- boolean equals(Object obj)로 정의된 equals 메서드는 기본적으로 2개의 객체가 동일한지 검사하기 위해 사용된다. 
- equals가 구현된 방법은 2개의 객체가 참조하는 것이 동일한지를 확인하는 것이며, 이는 동일성(Identity)을 비교하는 것이다.
- 즉, 2개의 객체가 가리키는 곳이 동일한 메모리 주소일 경우에만 동일한 객체가 된다.

## 비교 연산자와 equals() 메서드의 차이

### == (비교 연산자)

- `==` 연산자는 주로 두 개의 객체 주소의 값을 비교한다.([[참조 타입(Reference Type)]], Call By Reference)
- 주소의 값이란 실제 내용의 값이 아닌 자료의 위치의 값이다
- 즉, 두 객체가 메모리 상에서 같은 위치를 참조하고 있는지를 검사한다.
### equals() 메서드

- equals() [[메서드(Method)]]는 [[객체(Object)]]의 내용 비교한다.([[원시 타입(Primitive Type)]], Call By value)

```java
package joon;

public class codeTest {
    public static void main(String[] args) throws Exception{
    
        String str1 = "abc";
        String str2 = str1;
        String str3 = new String("abc");
        
        // == 연산자는 주소를 비교
        System.out.println(str1 == str2); // true
        // str2 에 st1 값을 넣었으므로 주소를 같이 공유
        
        System.out.println(str1 == str3); // false
        // str1 과 str3는 각각 생성 되었으므로 주소가 다름
        
        // equals()는 객체의 내용(문자열)을 비교함
        System.out.println(str1.equals(str2)); // ture
        System.out.println(str1.equals(str3)); // true
        // 내용을 비교하기떄문에 abc 내용이 같으므로 true 가 반환
    }
}
```

- 따라서 문자열을 비교할때에는 == 보다는 equals를 사용해야 한다.

## 동등성

- 프로그래밍을 하다보면 동일한 객체가 메모리 상에 여러 개 띄워져있는 경우가 있다. 

- 해당 [[객체(Object)]]는 서로 다른 메모리에 띄워져있으므로 동일한(Identity) [[객체(Object)]]가 아니다.
- 하지만 프로그래밍 상으로는 같은 값을 지니므로 같은 객체로 인식되어야 하는데, 이러한 동등성(Equality)를 위해 우리는 값으로 객체를 비교하도록 equals 메서드를 [[오버라이딩(Overriding)]]해주는 것이다.

- 예를 들어 아래와 같이 동일한 값을 갖는 문자열을 2개 생성하면 각각은 서로 다른 메모리에 할당되므로 동일하지 않다.(힙 영역에 생성된다.)
- 대신 같은 값([[원시 타입(Primitive Type)]]을 지니므로 동등하다.

- 하지만 동일성을 비교하는 equals 메서드를 호출해보면 true가 나오는데, 그 이유는 [[String 클래스에서 equals 메소드를 오버라이드하여 객체가 같은 값을 갖는지 동등성(Equality)을 비교하도록 처리했기 때문이다.

### equals와 hashCode의 관계

- 동일한 객체는 동일한 메모리 주소를 갖는다는 것을 의미하므로, 동일한 객체는 동일한 해시코드를 가져야 한다.
- 그렇기 때문에 우리가 equals() [[메서드(Method)]]를 [[오버라이딩(Overriding)]]한다면, hashCode() [[메서드(Method)]]도 함께 [[오버라이딩(Overriding)]] 되어야 한다.
 
- 이러한 equals와 [[hashCode()]]의 관계를 정의하면 다음과 같다.
	- Java 프로그램을 실행하는 동안 equals에 사용된 정보가 수정되지 않았다면, hashCode는 항상 동일한 정수값을 반환해야 한다. (Java의 프로그램을 실행할 때 마다 달라지는 것은 상관이 없다.)
	- 두 객체가 equals()에 의해 동일하다면, 두 객체의 hashCode() 값도 일치해야 한다.
	- 두 객체가 equals()에 의해 동일하지 않다면, 두 객체의 hashCode() 값은 일치하지 않아도 된다.

- 즉, obj1.equals(obj2) == True 이면 hashCode(obj1) == hashCode(obj2) 이여야하지만 hashCode(obj1) == hashCode(obj2) 라고 obj1.equals(obj2) == True일 필요는 없다.
- 하지만 우리는 다른 객체에 대해 동일한 hashCode를 생성한다면 hashTable을 생성하는데 불이익을 받을 수 있음을 인지하고 있어야 한다.


## equals와 hashCode의 Override

### equals() Override의 필요성

- 만약 아래와 같은 Employee 클래스가 있다고 하자. 
- Employee는 id를 고유값으로 갖는다.

```java
public class Employee{
    private Integer id;
    private String firstname;
    private String lastName;
    private String department;
 
    //Setters and Getters
}
```

- 만약 아래와 같이 같은 id 값을 갖는 2개의 Employ를 서로 다른 처리 과정에 의해 얻었다고 하자. 
- 2개의 Employee는 같은 id를 갖기 때문에 equals 연산을 한다면 true를 반환해야 한다.
- 하지만 아래의 예제는 깊게 볼 필요도 없이 false를 반환할 것이다.

```java
public class EqualsTest {
    public static void main(String[] args) {
        Employee e1 = new Employee();
        Employee e2 = new Employee();
        
        e1.setId(100);
        e2.setId(100);
        
        System.out.println(e1.equals(e2));  //false
    }
}
```

- 이러한 문제를 해결하기 위해 우리는 Employ에 다음과 같은 equals 메서드를 [[오버라이딩(Overriding)]] 해야 한다.

```java
public boolean equals(Object o) {
    if(o == null) {
        return false;
    }
    if (o == this) {
        return true;
    }
    if (getClass() != o.getClass()) {
        return false;
    }
     
    Employee e = (Employee) o;
    return (this.getId() == e.getId());
    
}
```

- 이제 equals에 의한 문제는 해결된 것 처럼 보인다. 
- 하지만 우리가 Employee를 [[HashSet]]과 같은 자료구조에 저장하려고 하면 또 다른 문제가 생기게 된다.

### hashCode() Override의 필요성

- 앞서 설명한대로 HashTable이나 [[HashSet]], [[HashMap]]과 같은 자료구조는 자료를 저장하기 위한 위치를 선택하기 위해 [[hashCode()]]를 이용한다.

- 그렇다면 우리가 수정한 Employee를 HashSet에 저장하면 어떤 결과가 나올까?

```java
import java.util.HashSet;
import java.util.Set;
 
public class EqualsTest {
    public static void main(String[] args) {
        Employee e1 = new Employee();
        Employee e2 = new Employee();
        
        e1.setId(100);
        e2.setId(100);
        
        System.out.println(e1.equals(e2)); // true
        
        Set<Employee> employees = new HashSet<Employee>();
        employees.add(e1);
        employees.add(e2);
         
        System.out.println(employees);  //Prints two objects
    }
}
```

- Object 클래스의 hashCode() 메소드는 해당 메모리 주소값을 반환한다고 설명하였다. 
- 그렇기 때문에 위의 e1과 e2는 다른 해시값을 반환할 것이고, HashSet에는 2개의 객체가 서로 다른 위치에 저장될 것이다.

- 우리는 이러한 문제를 해결하기 위해 hashCode 메소드도 Employee 클래스에 오버라이드하여 수정해주어야 한다.

```java
@Override
public int hashCode() {
    final int PRIME = 31;
    int result = 1;
    result = PRIME * result + getId();
    return result;
}
```

- 이러한 작업을 모두 거치면 hashSet에도 1개의 데이터만 저장되게 된다.

## equals로 null 체크 하는 방법

```java
package joon;

public class codeTest {
    public static void main(String[] args) throws Exception{
    
        String str1 = "2";
        String str2 = "";
        
        if(!"".equals(str1)){
            System.out.println("공백이 아닙니다.");
        }
        
        if("".equals(str2)){
            System.out.println("공백입니다.");
        }
    }
}
```