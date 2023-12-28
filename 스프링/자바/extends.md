- 상속은 클래스 정의 다음에 extends를 써주고 [[부모 클래스(super class)]] 이름을 적어주면 된다.  

### 문법
-  [[자식 클래스(sub class)]] extends [[부모 클래스(super class)]] 

- 코드 예시
```java
class Calculator { // 부모 클래스
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
	
class SubstractionableCalculator extends Calculator { // 상속    
	public void substract() {
		System.out.println(this.left - this.right);    
	}
} 
		
class MultiplicationableCalculator extends Calculator { // 상속
	public void multiplication() {
		System.out.println(this.left * this.right);    
	}
} 
	
class DivisionableCalculator extends MultiplicationableCalculator {  //상속
	public void division() {
		System.out.println(this.left / this.right);    
	}
} 
		
public class CalculatorDemo1 {     
	public static void main(String[] args) {
		SubstractionableCalculator c1 = new SubstractionableCalculator();
				
		c1.setOprands(10, 20); 
		c1.sum();     
		c1.avg();       
		c1.substract();
				
		DivisionableCalculator c2 = new 
		DivisionableCalculator();      
		
		c2.setOprands(10, 20);
		c2.sum();
		c2.avg();
		c2.multiplication();        
		c2.division();    
	}
}
```

- 서브클래스인 SubstractionableCalculator는 Calculator에서 정의한 [[메서드(Method)]] setOprands, sub, avg를 사용할 수 있다.

- 또한 상속 받은 MultiplicationableCalculator를 또 상속 받는거도 가능하다.                ex) DivisionableCalculator

→ 결과적으로  [[자식 클래스(sub class)]]의 [[인스턴스(Instance)]]를 생성하면 [[부모 클래스(super class)]]의 멤버와 자식 클래스의 멤버가 합쳐진 하나의 인스턴스로 생성된다.