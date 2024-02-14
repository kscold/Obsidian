- 기존의 [[Runnable]] [[인터페이스(Interface)]]는 결과를 반환할 수 없다는 한계점이 있었다. 

- 반환값을 얻으려면 공용 메모리나 파이프 등을 사용해야 했는데, 이러한 작업은 상당히 번거롭다. 
- 그래서 [[Runnable]]의 발전된 형태로써, Java5에 함께 추가된 제네릭을 사용해 결과를 받을 수 있는 Callable이 추가되었다. 

```java
@FunctionalInterface // 함수형 인터페이스라고 명시한 어노테이션
public interface Callable<V> {
    V call() throws Exception;
}
```