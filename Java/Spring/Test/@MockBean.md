- 따라서 [[어노테이션(Annotation)]]은 말 그래도 가짜 [[객체(Object)]]로 해당 단위 테스트에만 집중할 수 있도록 도와준다. 




## 예시

- 밑의 코드에서는 서비스를 MockBean으로 선언하였고, 서비스 내 의존성 연결고리를 신경 안써도 되며 서비스의 호출, 결과를 임의로 조작하여 테스트를 지원한다.

```java
@MockBean
private ContentService contentService;
```

```

