
- 클래스 메소드([[정적 메서드(Static Method)]])의 경우 [[인스턴스 변수(Instance Variable)]] 또는 [[인스턴스 메서드(instance method)]]를 사용할 수 없다.

- 또한, 클래스 메서드(정적 메서드)는 [[인스턴스(Instance)]] 생성 없이 호출이 가능하다.
- 반대로, [[인스턴스 메서드(instance method)]]에서는 [[인스턴스 변수(Instance Variable)]]를 사용할 수 있다.

## 예시

```java
class Car {
    public static String str1 = "클래스 변수";
    public String str2 = "인스턴스 변수";
    
    public static void ClassMethod() { // 정적 메서드(클래스 메서드)
    System.out.println(str1); // 클래스 변수 사용 가능
    // System.out.println(str2); 인스턴스 변수 사용 불가능
    }
    
    public void InstanceMethod() { // 인스턴스 메서드
    System.out.println(str1); // 클래스 변수 사용 가능
    System.out.println(str2); // 인스턴스 변수 사용 가능
	}
}

public class CarTest2 {
    public static void main(String[] args) {
    System.out.println(Car.str1); // 클래스 변수 바로 사용 가능
    // System.out.println(Car.str2); // 인스턴스 변수 바로 사용 불가능
    
    Car.ClassMethod(); // 클래스 메서드 바로 사용 가능
    // Car.InstanceMethod(); // 인스턴스 메서드 바로 사용 불가능
	    
        // 객체는 클래스 변수와 클래스 메서드를 공유한다.
        Car car1 = new Car();
        Car car2 = new Car();
		
		// 클래스 변수는 모든 메모리를 공유
        // 따라서 객체를 통해 수정해도 다른 객체나 직접 접근해도 수정된 결과를 출력
        car.str1 = "클래스 변수입니다."; // 객체를 통해 클래스 변수 변경
        System.out.println(car1.str1); // 클래스 변수입니다. 출력
        System.out.println(car2.str1); // 클래스 변수입니다. 출력
        System.out.println(car.str1);  // 클래스 변수입니다. 출력
		
        // 반면, 인스턴스 변수는 독립적인 메모리를 가지고 있다.
        // 변경한 인스턴스만 수정된 상태로 출력
        car1.str1 = "인스턴스 변수입니다.";
        System.out.println(car1.str2); // 인스턴스 변수입니다. 출력
        System.out.println(car2.str2); // 인스턴스 변수 출력

    }
}
```