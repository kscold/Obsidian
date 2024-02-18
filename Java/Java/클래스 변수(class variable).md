- [[멤버 변수(Nember Variable)]]에 포함된다.
- 클래스 변수는 [[인스턴스 변수(Instance Variable)]]와는 다르게 공통된 저장공간을 공유한다.
- 즉, 한 [[클래스(Class)]]로부터 생성되는 모든 [[인스턴스(Instance)]]들이 특정한 값을 공유해야 하는 경우에 사용한다.

- 선언할 때 [[static]] 키워드를 사용한다.

- 클래스 변수는 [[인스턴스(Instance)]]를 생성하지 않고도 "클래스명.클래스변수명"을 통해 사용이 가능하다.
- 클래스는 메타데이터 영역 또는 클래스 영역에 저장된다.

```java
class Variable {
    static int classVariable = 10; // 클래스 변수 초기화

    public static void main(String[] args) {
        System.out.println(Variable.classVariable); // 10 출력
        // 클래스명.클래스변수병으로 전역 필드로 바로 접근
}
```
