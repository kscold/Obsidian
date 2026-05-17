- 빌더 [[패턴(Pattern)]]은 [[객체(Object)]]에 생성을 위한 방법 중 하나로, [[객체(Object)]] 생성시 여러 [[Java/필드(Field)|필드(Field)]]가 존재할 때 그것의 순서에 의해 생기는 문제나 명시적이지 못한 [[생성자(constructor)]] 여러개에 의해 발생하는 문제를 해결하기 위해 나온 패턴이다.


## 예시

- 아래와 같은 방식으로 사용한다.

```java
public class Test{
    private Integer number;
    private String str;
    
    public static class Builder{
        private Integer number;
        private String str;
        
        public Builder number(Integer number){
            this.number = number;
            return number;
        }
        
        public Builder str(String str){
            this.str = str;
            return this;
        }
        
        public Test build(){
            return new Test(number, str);
        }
    }
}

public class TestMain{
   @Test
   void test1(){
       Test test = Test.builder()
                       .number(1)
                       .str("test")
                       .build();
       System.out.println("output : " + test.getNumber() + " " + test.getStr());
       
       Test test2 = Test.builder()
                       .str("test2")
                       .number(2)
                       .build();
       System.out.println("output : " + test2.getNumber() + " " + test2.getStr());
   }
}

// >> output : 1 test
// >> output : 2 test2
```

- 위와 같이 builder 패턴을 이용하게 되면, [[생성자(constructor)]]의 [[Java/필드(Field)|필드(Field)]] 순서를 알 필요 없이 필드명을 통해서 새로운 [[객체(Object)]]를 생성할 수 있고 더욱 명시적으로 객체 생성이 손쉽게 가능해진다.

- 하지만, 여기서 생기는 문제는 매 객체마다 builder를 만드는 것은 꽤나 수고롭고 반복적인 작업이라는 것이다.
- 이것을 해결할 수 있는 방법 중 하나가 [[롬복(lombok)]]의 [[@Builder]] [[어노테이션(Annotation)]]이다.