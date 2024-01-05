필드는 [[클래스(Class)]]에 포함된 [[변수(Variable)]]를 의미한다. 즉, [[객체(Object)]]의 속성을 정의하는 공간이다.

### 자바에서 변수 종류 
- 클래스 변수(cv: class variable)
- [[인스턴스(Instance)]] 변수(iv, instance variable)
- 지역 변수(lv, local variable)

필드에서 다루는 변수는 클래스 변수(cv)와 인스턴스 변수(iv)이다. 이 둘은 [[static]] 키워드를 통해 구분할 수 있다.

**자바의 변수 종류**

클래스 변수 : 필드 내에서 static 키워드와 함께 선언된 변수, static 변수 또는 공유 변수라고도 함

인스턴스 변수 : 필드 내에서 선언된 변수 (static 키워드 없음)

지역 변수 : 메서드 내에 포함된 모든 변수

```java
class Variable {
    static int classVariable; // 클래스 변수(static 변수 또는 공유변수)
    int instanceVariable;     // 인스턴스 변수

    void variableMethod() {
        int localVariable;      // 지역변수, 메서드 내에서만 사용 가능
    }
}
```

세 가지 유형의 변수들은 서로 다른 유효 범위(scope)