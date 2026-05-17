- @Target [[어노테이션(Annotation)]]은 사용자가 만든 [[어노테이션(Annotation)]]이 부착될 수 있는 타입을 지정하는 것이다.

- 그래서 @Target이라는 [[어노테이션(Annotation)]]은 annotation에만 부착될 수 있다.  

## ANNOTATION_TYPE 

- @Target에는 ANNOTATION_TYPE이 들어간다. 
- 타겟이 들어 갈 수 있는 내용은 밑에 표를 보면 아래와 같다.

| value |  |
| ---- | ---- |
| ANNOTATION_TYPE | [[어노테이션(Annotation)]] |
| CONSTRUCTOR | [[생성자(constructor)]] |
| FIELD | [[Java/필드(Field)\|필드(Field)]] 선언 ([[열거(Enum)]] 정수 포함) |
| LOCAL_VARIABLE | 로컬 [[변수(Variable)]] |
| METHOD | [[메서드(Method)]] |
| PARAMETER | 매개변수 |
| PACKAGE | [[패키지(package)]] |
| TYPE | [[클래스(Class)]], [[인터페이스(Interface)]] ([[어노테이션(Annotation)]]을 포함), [[열거(Enum)]] |

## 예시 

- 밑에 사진은 Target annotation을 구현한 부분이다.  
- value에는 ElementType이라는 [[열거(Enum)]]객체를 사용한다.

```java
@Target(ElementType.ANNOTATION_TYPE)
public @interface Target {
	ElementType[] value();
}
```

- 밑의 코드는 TestAnnotation에 Target annotation을 붙혀 위에 표와같이 METHOD, FIELD, ANNOTATION_TYPE, CONSTRUCTOR, PARAMETER, TYPE_USE에서 사용할 수 있게 만들었다.  

```java
@Target({METHOD, FIELD, ANNOTATION_TYPE, CONSTRUCTOR, PARAMETER, TYPE_USE})
public @interface TestAnnotaion {

}
```

- 밑 코드는 TestAnnotaion을 사용하여 만든 코드이다  

```java
public class test_main {
	@TestAnnotation
	private String a;
	
	@TestAnnotation
	public test_main(){
	
	}
	
	@TestAnnotation
	public Boolean b(@TestAnnotaion int bb){
		return true;
	}
}

@TestAnntaion
@interface TestAnnotaion_1{

}
```

- 위와 같이 Class Field Method Constructor Annotation에 TestAnnotation을 붙힐 수 있는 것을 알 수 있었다.
