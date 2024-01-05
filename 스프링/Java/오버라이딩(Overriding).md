- 부모 [[클래스(Class)]]로부터 상속받은 [[메서드(Method)]]의 내용을 재정의(변경) 하는 것을 오버라이딩이라고 한다. 

- 물론 상속받은 메소드를 그대로 사용해도 되지만 자식클래스에서 변경해야 하는 경우가 많다. 이렇게 변경이 이루어지는 경우에 부모메서드를 오버라이딩한다.

### 오버라이딩 사용 조건

오버라딩은 메소드를 새로 만들게 아니고 내용만을 새로 작성하는 것이다.
메소드의 선언부는 부모와 완전히 일치해야 하기 때문에 다음과 같은 조건을 만족해야한다.

1. 자식 클래스의 오버라이딩 하려는 메서드는 부모 클래스의 메서드와

- 이름이 같아야 한다.
- 매개변수가 같아야 한다.
- 반환타입이 같아야 한다.  

2. [[public]]는 조상클래스의 메소드보다 좁은 범위로 변경할 수 없다.  

- ex) 부모클래스 : public void xxx( ){ . . . }  , 자식클래스 : protectecd void xxx( ){ . . . } // 에러!!  

- 참고:  public -> protecd, -> (default), -> private 접근 범위가 좁아짐

3. 부모 클래스의 메소드보다 많은 수의 예외를 선언할 수 없다.  

- ex) 부모클래스 : void xxx ( ) throws IOException, SQLException { . . . }  
        자식클래스:  void xxx ( ) throws Exception { . . . }  
-  단순 개수 문제가 아님. Exception 은 모든 예외의 최고 부모이다. 가장 많은 개수의 예외를 던질 수 있다.  
   
4. [[인스턴스 메소드]]를 [[클래스 메소드]]([[static]]) 또는 그 반대로 변경할 수 없다.  


밑에 코드는 간단하게 오버라이딩을 나타낸 예시

```java
class 새로운 클래스명 extend 원래 클래스명
```
위에서 `extend`는 클래스 상속을 받았다는 것을 나타냄
[[@Override]] 어노테이션을 사용하여 오버라이딩 에러를 방지함

```java
class Car{

    void drive(){
        System.out.println("기름을 써서 출발");
    }
}

class EvCar extends Car{ // Car로부터 상속받음
    @Override // 어노테이션을 이용하여 상속받은 것 이상 불가능
    void drive(){
        System.out.println("전기를 써서 출발");
    }
}

class overrridingEX {
    public static void main(String[] args) {

        Car car = new Car();
        car.drive();

        EvCar evCar = new EvCar();
        evCar.drive();
        
    }
}

// 실행결과
기름을 써서 출발
전기를 써서 출발
```
