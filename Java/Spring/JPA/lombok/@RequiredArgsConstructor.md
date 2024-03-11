- [[생성자(constructor)]] 주입의 단점은 위의 Constructor(생성자) 코드처럼 생성자를 만들기 번거롭다는 것이다.
- 하지만 이를 보완하기위해 [[롬복(lombok)]]을 사용하여 간단한 방법으로 생성자 주입 방식의 코딩을 할 수 있다.

## @RequiredArgsConstructor

- [[final]]이 붙거나 @NotNull이 붙은 [[Java/필드(Field)|필드(Field)]]의 [[생성자(constructor)]]를 자동 생성해주는 [[롬복(lombok)]] [[어노테이션(Annotation)]]이다.

- [[Java/필드(Field)|필드(Field)]] 주입방식을 사용한 기존 [[서비스(Service)]]이다.

```java
@Service
    public class BannerServiceImpl implements BannerService {
	    
        @Autowired
        private BannerRepository bannerRepository;
	    
        @Autowired
        private CommonFileUtils commonFileUtils;
```

### @RequiredArgsConstructor 를 활용한 생성자 주입

```java
    @Service
    @RequiredArgsConstructor
    public class BannerServiceImpl implements BannerService {
    
        private final BannerRepository bannerRepository;
    
        private final CommonFileUtils commonFileUtils;
        ...
```

- @RequiredArgsConstructor를 사용하지 않으면 밑의 코드와 같이 생성자 주입을 해야한다.

```java
    @Service
    public class BannerServiceImpl implements BannerService {
	    
        private BannerRepository bannerRepository;
	    
        private CommonFileUtils commonFileUtils;
	    
        @Autowired
        public BannerServiceImpl(BannerRepository bannerRepository, CommonFileUtils commonFileUtils) {
            this.bannerRepository = bannerRepository;
            this.commonFileUtils = commonFileUtils;
        }
        ...
```