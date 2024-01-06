- 클래스간 상속에 사용되는 키워드이다.
- [[클래스(Class)]] 상속은 클래스 정의 다음에 extends 키워드를 써주고 [[부모 클래스(super class)]] 이름을 적어주면 된다.  
- extends로 인한 [[다중상속]]은 자바에서 금지되어 있다.
### 문법

 ```java
subClass extends superClass
```

- 코드 예시
```java
class Calculator { // 최초 부모 클래스 정의
	int left, right;     
	
	public void setOprands(int left, int right) {        
		this.left = left;        
		this.right = right;    
	}     
	
	public void sum() {      
		System.out.println(this.left + this.right);    
	}     
	
	public void avg() {        
		System.out.println((this.left + this.right) / 2);    
	}
} 

class SubstractionableCalculator extends Calculator { // 부모 클래스 상속    
	public void substract() { // 부모 클래스를 오버라이딩
		System.out.println(this.left - this.right);    
	}
}

class MultiplicationableCalculator extends Calculator { // 부모 클래스 상속
	public void multiplication() { // 부모 클래스를 오버라이딩
		System.out.println(this.left * this.right);    
	}
} 

	class DivisionableCalculator extends MultiplicationableCalculator { // 자식클래스(부모 클래스)를 또 다른 자식 클래스로 상속
	public void division() { // 부모 클래스를 오버라이딩
		System.out.println(this.left / this.right);    
	}
} 

public class CalculatorDemo1 {     
	public static void main(String[] args) {
		SubstractionableCalculator c1 = new SubstractionableCalculator();
		
		c1.setOprands(10, 20); // 계산할 기본 값 설정
		c1.sum(); // 부모 클래스에 정의된 더하기
		c1.avg(); // 부모 클래스에 정의된 평균구하기
		c1.substract(); // 자식 클래스에서 오버라이딩된 빼기
		
		DivisionableCalculator c2 = new DivisionableCalculator();      
		
		c2.setOprands(10, 20);
		c2.sum(); // 부모 클래스에 정의된 더하기
		c2.avg(); // 부모 클래스에 정의된 평균구하기
		c2.multiplication(); // 자식 클래스에서 상속된 자식클래스에서 오버라이딩된 곱하기      
		c2.division(); // 자식 클래스에서 오버라이딩된 나누기
	}
}
```

- [[자식 클래스(sub class)]](서브클래스)인 SubstractionableCalculator는 Calculator에서 정의한 [[메서드(Method)]] setOprands, sub, avg를 사용할 수 있다.

- 또한 상속 받은 MultiplicationableCalculator를 또 상속 받는거도 가능하다.(예시 DivisionableCalculator)

→ 결과적으로  [[자식 클래스(sub class)]]의 [[인스턴스(Instance)]]를 생성하면 [[부모 클래스(super class)]]의 멤버와 자식 클래스의 멤버가 합쳐진 하나의 인스턴스로 생성된다.