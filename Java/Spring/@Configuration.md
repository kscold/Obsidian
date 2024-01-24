- [[스프링 컨테이너(Spring Container)]]가 해당 [[어노테이션(Annotation)]]이 붙은 [[클래스(Class)]]를 설정 정보로 사용한다.
- 클래스 내부에 [[@Bean]] 어노테이션이 적힌 [[메서드(Method)]]를 모두 호출하여 얻은 [[객체(Object)]]를 스프링 컨테이너에 등록하게 된다.

- 스프링 컨테이너에 등록된 객체를 스프링 [[Bean]]이라 한다.

## 문법

```java
applicationContext.getBean(”이름”, 타입);
```

- 스프링 빈은 getBean() 메서드를 이용하여 얻을 수 있다.