
this 키워드는 [[클래스(Class)]]가 [[인스턴스(Instance)]]화 되었을때의 자기자신의 메모리 주소를 담고있는 키워드이다.

객체 자기자신의 메모리 주소를 담고있으므로 도트연산자(.)를 이용해 접근하여 멤버 [[변수(Variable)]]와 [[메서드(Method)]]를 사용할 수 있다.


밑에는 this를 통해 멤버변수([[클래스 변수]]와 [[인스턴스 변수]])나 메소드에 접근하여 사용하는 예시
```java
public class User {
    private Integer id;
    protected String account;
    public String password;

    public void printId() {
        System.out.println(this.id);
    }
    
    public void printAll() {
        this.printId();
        System.out.println(this.account);
        System.out.println(this.password);
    }
}
```

위 코드를 보면 this를 통해 멤버변수나 메소드에 접근하여 사용하는 것을 확인할 수 있다.

### this()

클래스 내부에서 this()를 호출하면 **생성자**를 호출한다.  
물론 매개변수가 있는 생성자라면 그에 맞게 인자를 넣어주어 호출하면 된다.

밑에 코드는 매개변수가 하나인 생성자에서 매개변수가 3개인 생성자를 호출하는 예시

```java
public class User {
    private Integer id;
    protected String account;
    public String password;

    public User(Integer id, String account, String password) {
        this.id = id;
        this.account = account;
        this.password = password;
    }

    public User(Integer id) {
        this(id, "a", "b");
    }
}
```


this()를 통해 생성자를 호출할때는 다음의 2가지 제약이 있다.  
1. 생성자에서만 호출가능하다.  
2. 제일 첫 문장에서 호출해야한다.  
3. 생성자 자기 자신을 호출할 수 없다.(재귀호출이 불가능하다.)

