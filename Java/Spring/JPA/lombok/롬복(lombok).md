- 코드를 간소화해주는 라이브러리로 개발을 하다 보면 [[Getter and Setter]]나 [[생성자(constructor)]], [[toString()]]과 같은 필수 메서드를 사용하기 마련인데 이 코드들을 간편하게  [[어노테이션(Annotation)]]을 통해 작성할 수 있도록 만들어준다.

- 프로그램의 수행 과정을 기록으로 남기는 [[로깅(logging)]] 기능을 통해 println()문을 개선할 수 있다.

### 롬복 설정

```
dependencies {  
    compileOnly 'org.projectlombok:lombok'  
    annotationProcessor 'org.projectlombok:lombok' 
    // 이 두줄을 build.gradle에 추가한다.
    ...
}
```

### 롬복의 종류

- [[@AllArgsConstructor]] : 자동으로 생성자를 만들어준다.
- [[@ToString]] : 자동으로 toString() 메서드를 대체해준다.
- [[@SIf4j]]: 이 어노테이션을 통해 로깅 기능을 사용할 수 있다.