- 자바스크립트에서 클래스는 특정 [[객체(Object)]]를 생성하기 위해 [[변수(Variable)]]와 [[메서드(Method)]]드를 정의하는 일종의 틀로, [[객체(Object)]]를 정의하기 위한 상태([[state]])와 메서드([[함수(Function)]])로 구성된다.

- 자바스크립트는 사실 일반적인 클래스 개념이 아닌 프로토타입([[prototype]]) 기반의 언어이다.
- es6부터 추가된 class [[키워드(Keyword)]]는 직관적으로 쉽게 코드를 읽을 수 있게 만들어 줄 뿐만 아니라, 작성하기도 쉽고 또 class 기반 언어에 익숙한 개발자가 더 빠르게 적응할 수 있다.

## 클래스 사용 이유

- 클래스를 사용하지 않아도 [[new]] 연산자와 [[생성자(Constructor)]] 함수인 new function을 사용할 수 있다.
- 그러나 실무에선 사용자나 물건같이 동일한 종류의 [[객체(Object)]]를 여러 개 생성해야 하는 경우가 잦다.
- 따라서 클래스(class) 문법을 사용하면 객체 지향 프로그래밍에서 사용되는 다양한 기능을 자바스크립트에서도 사용할 수 있다.

- 밑의 예시는 클래스 키워드를 사용하지 않고 [[new]]를 이용해 [[객체(Object)]]를 만드는 코드이다.
- [[prototype]]
```javascript
function Me(name) {
  this.name = name;
}

Me.prototype.wow = function () {
  console.log("WOW!");
};

let person = new Me("Jason"); // new를 통해 새로운 객체를 생성
person.wow(); // WOW!
```

- 밑의 예시는 클래스 키워드를 사용한 코드이다.
- [[생성자(Constructor)]]인 [[constructor()]] 함수를 통해 [[this]]로 [[인스턴스(Instance)]] [[변수(Variable)]]을 만들어 줬다.

```javascript
class Me {
  constructor(name){ // 생성자 선언
    this.name = name;
  }
  
  wow(){
    console.log("WOW!");
  }
}
  
let person = new Me("Jason");
person.wow() // WOW!
```

## class 살펴보기

```javascript
class Korean {
  constructor(name, age) {
    this.name = name;
    this.age = age;
    this.country = 'Korea';
  }

  addAge(age) {  
    return this.age + age;
  }
}
```

- 클래스 내에 정의된 [[함수(Function)]]를 [[메서드(Method)]]라고 부른다.
- 클래스를 통해 생성된 [[객체(Object)]]를 [[인스턴스(Instance)]]라고 부른다.

- 클래스도 [[함수(Function)]]처럼 호출하기 전까지는 코드가 실행되지 않는다. 단지 `Korean`이라는 클래스를 정의만 했을 뿐이다.
- [[new]] [[키워드(Keyword)]]와 소괄호`()`를 사용하여 호출할 수 있다.

- 클래스 이름은 `Korean`과 같이 항상 대문자로 시작한다.

- [[constructor()]]는 `class`에서 필요한 기초 정보를 세팅하는 곳이다.
- [[객체(Object)]]를 [[new]]로 생성할 때 가장먼저 자동으로 호출된다.
    - [[constructor()]] [[메서드(Method)]]에서 `name`과 `age`, 2개의 매개변수로 `Korean` [[인스턴스(Instance)]]의 `name`, `age` [[속성(Property)]]에 값을 할당했다.
    
    - [[this]] 는 본인 [[객체(Object)]]를 의미한다. 
    - 클래스 내에서 메서드끼리 소통하기 위해서는 [[this]]가 필요하다.

```javascript
let kim = new Korean("KIMJINYOUNG",24);

{
	name: 'KIMJINYOUNG',
	age: 24,
	country: 'Korea',
	addAge: function(age){
		return this.age + age;  	
	}
}
```

`Korean` 클래스를 이용해 `kim` 객체를 만들면 위와 같은 instance가 생성된다.