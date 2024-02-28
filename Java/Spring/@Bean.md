- 스프링에서 [[Bean]]으로 등록할 때 사용하는 [[어노테이션(Annotation)]]이다.
- @Bean [[어노테이션(Annotation)]]의 경우 개발자가 직접 제어가 불가능한 외부 라이브러리등을 Bean으로 만들려할 때 사용된다. 

## 예시

```java
@Configuration
public class ApplicationConfig {
    @Bean
    public ArrayList<String> array(){
        return new ArrayList<String>();
    }   
}
```

- 위는 @Bean [[어노테이션(Annotation)]]을 이용하여 Bean을 생성한 예시이다.
- 위와 코드와 ArrayList같은 라이브러리등을 Bean으로 등록하기 위해서는 별도로 해당 라이브러리 객체를 반환하는 [[메서드(Method)]]를 만들고 @Bean어노테이션을 붙혀주면 된다. 
- 이 경우 Bean어노테이션에 아무런 값을 지정하지 않았으므로 [[메서드(Method)]] 이름을 CamelCase로 변경한 것이 Bean id로 등록된다. 

- 위의 코드처럼 [[메서드(Method)]] 이름이 arrayList()인 경우 arrayList가 Bean id가 된다.

```java
@Configuration
public class ApplicationConfig {    
    @Bean(name="myarray")
    public ArrayList<String> array(){
        return new ArrayList<String>();
    }   
}
```

- 위와 같이 @Bean 어노테이션에 name이라는 값을 이용하면 자신이 원하는 id로 Bean을 등록할 수 있다. 
- 어노테이션 안에 값을 입력하지 않을 경우 [[메서드(Method)]]의 이름을 CamelCase로 변경한것이  Bean의 id가 된다. 


```java
@Configuration
public class ApplicationConfig {    
    @Bean
    public ArrayList<Integer> array(){
        return new ArrayList<Integer>();
    }   
	
    @Bean
    public Student student() {
        return new Student(array());
    }
}
```

- 의존관계가 필요할 때에는 위킈 Student [[객체(Object)]]의 경우 생성자에서 ArrayList를 주입 받도록 코드를 짜놓았다.
- 이 때 Bean으로 선언된 array()메소드를 호출함으로써 의존성을 주입([[DI(Dependency Injection)]])할 수 있다.