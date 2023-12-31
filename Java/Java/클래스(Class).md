- 자바에서 클래스(class)란 [[객체(Object)]]를 정의하는 틀 또는 설계도와 같은 의미로 사용한다.(붕어빵 틀)
- 클래스는 크게 일반 클래스와 클래스 자체에 abstract가 붙거나 클래스 내에 '[[추상 메서드(Abstract Method)]]'가 하나 이상 존재하는 [[추상 클래스(Abstract Class)]]로 나뉜다.

- 자바에서는 이러한 설계도인 클래스를 가지고, 여러 객체를 생성하여 사용하게 된다.

- 클래스는 객체의 상태를 나타내는 [[Java/필드(Field)]]와 객체의 행동을 나타내는 [[메서드(Method)]]로 구성된다.
- 즉, [[Java/필드(Field)]]란 클래스에 포함된 [[변수(Variable)]]를 의미한다.

- 또한, [[메서드(Method)]]란 어떠한 특정 작업을 수행하기 위한 명령문의 집합(함수)이라 할 수 있다.

[[private]]의 경우 클래스 내에서만 사용할 수 있게 주로 만든다.
```java
class product { // public, private가 없으므로 패키지(package) 내에서만 접근 가능
	private String name;
	private int price;

	void setName(String new_name){ // 인스턴스 메소드로 Getter와 Setter 사용
		name = new_name // this.name으로 써도 되고 안써도 됨
	}

	String getName() {
		return name;
	}

	void setPrice(String new_price){
		price = new_name
	}

	String getPrice() {
		return price;
	}
}
```


### [[Getter and Setter]]를 사용
- setName()메소드와 setPrice()메소드로 이름과 가격을 매개변수로 받아 대입하고, getName()메소드와 getPrice()메소드로 이름과 가격을 리턴받아 사용이 가능하다. 
- private를 사용하는 용도는 은닉([[캡슐화(encapsulation)]]) 때문이다.