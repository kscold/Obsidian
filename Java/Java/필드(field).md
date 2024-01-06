- 필드는 [[클래스(Class)]]에 포함된 [[변수(Variable)]]를 의미한다. 즉, [[객체(Object)]]의 속성을 정의하는 공간이다.
- [[멤버 변수]]와 같은 말이다.

### 자바에서 변수 종류 
- [[클래스 변수(cv, class variable)]] : 필드 내에서 static 키워드와 함께 선언된 변수, static 변수 또는 공유 변수라고도 한다.
- [[인스턴스 변수(iv, instance variable)]] :  필드 내에서 선언된 변수이다. (static 키워드가 없다.)
- [[지역 변수(lv, local variable)]] : [[메서드(Method)]] 내에 포함된 모든 변수를 의미한다.

- 필드에서 다루는 변수는 클래스 변수(cv)와 인스턴스 변수(iv)이다. 이 둘은 [[static]] 키워드를 통해 구분할 수 있다.

```java
class Variable {
    static int classVariable; // 클래스 변수(static 변수 또는 공유변수)
    int instanceVariable; // 인스턴스 변수 기본적으로 default

    void variableMethod() {
        int localVariable;      // 지역변수, 메서드 내에서만 사용 가능
    }
}
```

세 가지 유형의 변수들은 서로 다른 유효 범위(scope)