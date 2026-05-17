- Object 클래스가 가진 [[메서드(Method)]]이다.
- [[객체(Object)]]가 가지고 있는 정보나 값들을 문자열로 만들어 리턴하는 메서드이다.
- Spring boot VO(DTO)클래스에서 사용하는 toString 메소드는 Object클래스에 있는 toString() 메소드를 [[오버라이딩(Overriding)]]하여 사용(재정의)한다.

### 예시
- 이름과 나이를 멤버 변수로 가지는 Member라는 클래스가 있다.
```java
// DTO 혹은 VO 파일
public class Member {

    private String name;
    private int age;
    
    // Getter & Setter 선언
    public String getName() {  
	    return name;  
	}  
  
	public void setName(String name) {  
	    this.name = name;  
	}  
  
	public int getAge() {  
	    return age;  
	}  
  
	public void setAge(int age) {  
	    this.age = age;  
	}  
}
```

- Member의 정보를 눈으로 확인하고 싶은 경우 다음과 같은 방식을 사용할 것이다.

```java
System.out.println("Member [name=" + member.getName() + "] [age=" + member.getAge() + "]");
```

- 하지만 이 방법은 매번 코드를 작성해야 하는 번거로움이 존재한다.

- 따라서 이는 toString() 메서드를 재정의하여 재사용하는 방식으로 해결해줄 수 있다.

```java
// DTO 혹은 VO 파일에 추가
@Override
public String toString() {
    return "Member{" +
           "name='" + name + '\'' +
           ", age=" + age +
           '}';
}
```
