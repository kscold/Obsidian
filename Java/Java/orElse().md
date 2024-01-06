- [[Optional]]에 올 값이 null인 경우 orElse 안에 있는 내용을 실행시킨다.

```java
String isNull;
String name;
        
isNull = "loose";
name = Optional.ofNullable(isNull).orElse("test"); // null이 아니므로 loose 대입

System.out.println(name); //isNull값이 null이 아니므로 "loose" 출력

isNull = null;
name = Optional.ofNullable(isNull).orElse("test"); // null인 경우에 test 문자열을 반환

System.out.println(name); //isNull값이 null이므로 "test" 출력
```

- 여기서 [[ofNullable()]]를 사용하여 Optional이 null 일 때도 가능하게 만들었다.


- 그래서 orElse~는 if문을 이용해 처리해야 하는 명령어를 짧게 람다식처럼 처리할 수 있는 메소드라고 볼 수 있다.


#### [**orElse~의 룰(리턴 값 일치)**](https://stir.tistory.com/140#orElse-%EC%-D%--%--%EB%A-%B--%EB%A-%AC%ED%--%B-%--%EA%B-%--%--%EC%-D%BC%EC%B-%---)

```java
String name1 = Optional.ofNullable("test1").orElse("test2");
```

너무 당연한 것이지만 orElse를 사용할 때는 Optional의 대상이 되는 값과 orElse 내의 매개변수 리턴 값은 같아야 한다.

만약 같지 않다면 orElse가 아닌 if문을 이용해서 처리해야 한다.