- @Retention 애노테이션은 애노테이션의 [[생명 주기(Life Cycle)]] 즉, [[어노테이션(Annotation)]]이 언제까지 살아 남아 있을지를 정하는 것이다.

## RetentionPolicy

- @Retentio 어노테이션의 속성으로 RetentionPolicy 라는 것이 있다.

- 여기에 올 수 있는 값은 source, class, runtime 이렇게 3가지가 있다.

### RetentionPolicy.SOURCE 

- 소스 코드(.java)까지 남아있는다.
- [[Getter and Setter]] 같은 경우 [[롬복(lombok)]]이 바이트 코드를 생성해서 넣어주는 것이기 때문에, 굳이 바이트코드에 [[어노테이션(Annotation)]] 정보가 들어갈 필요가 없다.

### RetentionPolicy.CLASS

- 클래스 파일(.class)까지 남아있는다.
- 즉, 바이트 코드이다.

- 인텔리제이를 사용하다보면 @NonNull 등이 붙어있는 경우 null 값을 넣게되면 노랑색 경고로 알려준다.
- SOURCE로 사용해도 될 것 같지만, 중요한점은 Maven/Gradle로 다운받은 라이브러리와 같이 jar 파일에는 소스가 포함되어있지 않다는 점이다.

- .class 파일만 포함되어 있다.(Download Sources 옵션은 논외이다.)
- 즉, .class 파일만 존재하는 라이브러리 같은 경우에도 타입체커, IDE 부가기능 등을 사용할수 있으려면 CLASS 정책이 필요하게 된다.
- SOURCE 정책으로 사용한다면 컴파일된 라이브러리의 jar 파일에는 어노테이션 정보가 남아있지 않기 때문이다.

- 그외에도 클래스로딩시 무언가를 하고 싶은 경우에도 사용될 수도 있다.

### RetentionPolicy.RUNTIME

- 런타임에 [[어노테이션(Annotation)]] 정보를 뽑아 쓸수 있다는 의미다.
- 즉, [[리플렉션(Reflection)]] API 등을 사용하여 어노테이션 정보를 알수가 있다는 의미이다.
- 스프링을 예로 들자면, [[@Controller]], [[@Service]], [[@Autowired]] 등이 있다. 
- 스프링이 올라오는 실행 중인 시점에 [[컴포넌트(Component)]] 스캔이 가능해야하기 때문에 RUNTIME 정책이 필요하다.
- 스프링도 내부적으로 [[리플렉션(Reflection)]] 등을 활용하여 어노테이션이 붙은 놈들만 가져온다.


![](https://blog.kakaocdn.net/dn/blwAHp/btq0NvNGc0L/j2vDXl1a4qFhnCaGiOfMvk/img.png)