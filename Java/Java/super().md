- super()는 [[자식 클래스(sub class)]]에서 [[부모 클래스(super class)]]로부터 상속받은 멤버를 참조하는데 사용되는 참조 변수이다.

- 멤버변수와 지역변수의 이름이 같을 때 [[this()]]를 붙여서 구별한 것 처럼 상속받은 멤버와 자신의 [[클래스(Class)]]에 정의된 멤버의 이름이 같을 때 super를 붙여서 구별할 수 있다.

- 부모의 멤버(클래스, [[인스턴스(Instance)]] 포함)와 자신의 멤버를 구별하는데 사용된다는 점을 제외하고는 super와 this는 근본적으로 같다.

- 모든 [[인스턴스 메서드(instance method)]]소드에서는 자신이 속한 인스턴스의 주소가 지역변수로 저장이 되는데, 이것이 참조변수인 this와 super의 값이 된다.

- [[static]] [[메서드(Method)]]([[클래스 메서드]])는 [[인스턴스(Instance)]]와 관련이 없어 static 메서드에서는 사용이 불가하다.
즉, [[인스턴스 메서드(instance method)]]에서만 사용이 가능하다.

## [[부모 클래스(super class)]]의 [[생성자(constructor)]]  == super

this() 는 같은 클래스의 다른 생성자를 호출하는 데 사용되고, super( )는 부모 클래스의 생성자를 호출하는데 사용한다.

1. 자식 클래스의 인스턴스를 생성하면, 코드를 공유하기 때문에 멤버(필드 내의 요소들)가 모두 합쳐진 하나의 인스턴스가 생성이 된다. 

2. 이때 부모 클래스의 멤버의 초기화 작업이 수행되어야 하기 때문에 자식 클래스의 생성자에서도 부모클래스의 생성자가 호출되어야 한다.

Object 클래스를 제외한 모든 클래스가 생성자 첫 줄에 생성자 this( ) 또는 super( )를 호출해야 한다. 그렇지 않으면 컴파일러는 자동적으로 super( ); 를 생성자 첫 줄에 삽입한다.

밑에 코드는 사용방법의 예시
```java
class Car{ 
	int wheelDrive;
	
	public Car(int wheelDrive) { 
		this.wheelDrive = wheelDrive; 
	} 
	
	void drive(){
		System.out.println("기름을 써서 출발 구동은?= " + this.wheelDrive); 
	} 
} 

class EvCar extends Car{ // 부모 클래스인 Car로 부터 상속을 받음
	int chargeEV; // 상속받은 값 이외에도 값을 추가하기 위해 인스턴스 변수 선언
	
	public EvCar(int wheelDrive, int chargeEV) { 
		//super(); 에러! 부모클래스에 정의된 생성자가 없음 매개변수가 없기 때문에 오류
		super(wheelDrive); // 부모클래스의 생성자 Car(int wheelDrive) 호출 
		this.chargeEV = chargeEV; // 같은 클래스지만 다른 생성자를 호출
	}
	 
	@Override // 오버라이딩을 잘못하면 에러
	void drive(){ // 지역함수 선언
		System.out.println("전기를 써서 출발 " + "전기충전량= " + this.chargeEV + " 구동은?= " + this.wheelDrive); 
	} 
} 
	
class overrridingEX { 
	public static void main(String[] args) { // 클래스 매서드 정의
		Car car = new Car(2); //이륜구동 
		car.drive(); 
		EvCar evCar = new EvCar(4, 300); //사륜구동 
		evCar.drive(); 
	} 
}

// 결과

기름을 써서 출발 구동은?= 2

전기를 써서 출발 전기충전량= 300 구동은?= 4
```
