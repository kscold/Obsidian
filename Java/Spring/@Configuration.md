- 스프링에서 [[Bean]]을 수동으로 등록하기 위해서는, 설정 class위에 @Configuration을 추가하고, [[@Bean]]을 사용해 수동으로 빈을 등록할 수 있다.

- [[스프링 컨테이너(Spring Container)]]에 등록된 [[객체(Object)]]를 스프링 [[Bean]]이라 한다.
- [[클래스(Class)]] 내부에 [[@Bean]] 어노테이션이 적힌 [[메서드(Method)]]를 모두 호출하여 얻은 [[객체(Object)]]를 스프링 컨테이너에 등록하게 된다.
- 이때 [[메서드(Method)]] 이름으로 빈의 이름이 결정된다. 
- 그러므로 중복된 빈 이름이 존재하지 않도록 주의해야 한다.

- 즉, [[스프링 컨테이너(Spring Container)]]가 @Configuration [[어노테이션(Annotation)]]이 붙은 [[클래스(Class)]]를 설정 정보로 사용하고 [[싱글톤(Singleton)]]을 보장하기 위해 사용한다.


## getBean()

- 스프링 빈(Bean)은 getBean() 메서드를 이용하여 얻을 수 있다.

```java
applicationContext.getBean(”이름”, 타입);
```


## @Configuration의 역할

- @Configuration은 단순히  Bean을 수동 등록하기 위한 애노테이션이 아니다.

- [[Bean]]을 등록할 때 [[싱글톤(Singleton)]]이 되도록 보장해준다.
- [[스프링 컨테이너(Spring Container)]]서 [[Bean]]을 관리할 수 있게 된다.

- 우선 아래 코드와 같은 class가 있다고 가정한다.

```java
public class MyBean {

    public MyBean() {
        System.out.println("MyBean instance created");
    }
}
```

```java
public class MyBeanConsumer {

    public MyBeanConsumer(MyBean myBean) {
        System.out.println("MyBeanConsumer created");
        System.out.println("myBean hashcode = "+myBean.hashCode());
    }
}
```

- 사용할 설정 파일은 아래와 같다.

```java
@Configuration
public class AppConfig {

    @Bean
    public MyBean myBean() {
        return new MyBean();
    }
	
    @Bean
    public MyBeanConsumer myBeanConsumer() {
        return new MyBeanConsumer(myBean());
    }
}
```

- 위 코드를 실행하면 "MyBean instance created"가 총 몇 번 호출될까?
- Bean들은 싱글톤으로 등록되기 때문에, 다음과 같이 단 1번만 출력되는 것을 확인할 수 있다.

```java
MyBean instance created
MyBeanConsumer created
myBean hashcode = 1647766367
```

- 또한 Consumer도 싱글톤 빈 이기 때문에 한 번만 등록된다.
- @Configuration [[어노테이션(Annotation)]]을 제거하면 다음과 같이 출력된다.

```java
MyBean instance created
MyBean instance created
MyBeanConsumer created
myBean hashcode = 1647766367
```

- myBean() [[메서드(Method)]]가 호출될 때마다 new로 새로운 MyBean을 생성하기 때문에 "MyBean instance created"이 2번 호출되게 된다.

- 즉, @Bean만 사용해도 스프링 빈으로 등록은 되지만 싱글톤이 유지되지는 않는다.

## 예시

```java
@Configuration
public class SomeConfig {

    @Bean
    public ShineResource shine() {
        return new ShineResource();
    }

}
```

- 일반적으로 위와 같이 의존성 주입([[DI(Dependency Injection)]])을 위해서 @Configuration을 사용하게 된다.
- @Configuration 안에서 [[@Bean]]이 빈([[Bean]])으로 등록되는 과정은 간단하다. 

- [[스프링 컨테이너(Spring Container)]]는 @Configuration이 붙어있는 [[클래스(Class)]]를 자동으로 빈으로 등록하게 된다.


- @Configuration 내부를 보면 사실 [[@Component]]가 추가되어있다.
- 따라서 해당 Class 또한 Bean으로 등록된다.

- 이후 해당 [[클래스(Class)]]의 본문을 파싱 해서 [[@Bean]]이 붙어있는 [[메서드(Method)]]를 찾아서 빈([[Bean]])을 생성해준다.


- 다음은 아래의 테스트 코드를 살펴본다.

```java
@Test
void configurationDeep() {
    ApplicationContext ac = new AnnotationConfigApplicationContext(AppConfig.class);

    AppConfig bean = ac.getBean(AppConfig.class);
    System.out.println("bean = " + bean.getClass());
}
```

- 설정 파일로 넘긴 AppConfig.class 안에 있는 것들이 빈으로 등록된다. 
- 또한 AppConfig 자체도 빈([[Bean]])으로 등록이 된다.

- 위의 테스트 결과를 출력하면 예상되는 결과는 class hello.core.AppConfig이다.

![[Pasted image 20240226020200.png]]

- 그러나 테스트 결과를 보면 예상했던 결과와는 달리 뒤에 클래스 명에 xxxCGLIB가 붙으면서 상당히 복잡해진 것을 볼 수 있다.
- 만든 클래스가 아니라 스프링이 CGLIB라는 바이트코드 조작 라이브러리를 사용해서 AppConfig 클래스를 상속받은 임의의 Proxy 클래스를 만들고, 그 다른 클래스를 스프링 빈으로 등록한 것이다.


![[Pasted image 20240226020336.png]]

- 이 과정을 이미지로 보면 위에서 원래의 AppConfig를 상속한 proxy AppConfig가 빈으로 등록되며, [[싱글톤(Singleton)]]이 되도록 보장해 준다

- AppConfig@CGLIB 예상 코드는 다음과 같다.

```java
@Bean
public MyBean myBean() {
    if (myBean이 이미 스프링 컨테이너에 등록되어 있으면?) { 
        return 스프링 컨테이너에서 찾아서 반환;
    } else { // 스프링 컨테이너에 없으면 기존 로직을 호출해서 myBean을 생성하고 스프링 컨테이너에 등록 
        return 생성 후 반환
    } 
}
```

- [[@Bean]]이 붙은 [[메서드(Method)]]마다 이미 스프링 빈이 존재하면 존재하는 빈을 반환하고, 스프링 빈이 없으면 생성해서 스프링 빈([[Bean]])으로 등록하고 반환하는 코드가 동적으로 만들어진다. 
- 덕분에 [[싱글톤(Singleton)]]이 보장되는 것이다.