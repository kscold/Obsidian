- 코드의 재사용을 높이기 위해 나온 것이 바로 상속이다.
- [[클래스(Class)]]라는 개념이 있는 Java에서는 중복된 코드를 상속받아 코드 재활용을 할 수 있다.
- 하지만 초기 자바스크립트에서는 [[클래스(Class)]]가 없는, 프로토타입([[prototype]]) 기반 언어이다.
- 그래서 프로토타입을 이용하여 코드 재사용을 할 수 있다.

- 이 방법에도 크게 두 가지로 분류할 수 있다.
- classical 방식과 prototypal 방식이 있다.
- classical 방식은 [[new]] 연산자를 통해 생성한 객체를 사용하여 코드를 재사용 하는 방법이다.
- 마치 자바에서 [[객체(Object)]]를 생성하는 방법과 유사하여 classical 방식이라고 한다. 
- prototypal 방식은 리터럴 또는 Object.create()를 이용하여 객체를 생성하고 확장해 가는 방식이다.
- 두 가지 방법 중 자바스크립트에서는 prototypal 방식을 더 선호한다.
- 그 이유는 classical 방식보다 간결하게 구현할 수 있기 때문이다.


## 기본 방법

- 부모에 해당하는 함수를 이용하여 객체를 생성한다.
- 자식에 해당하는 함수의 prototype 속성을 부모 함수를 이용하여 생성한 객체를 참조하는 방법이다.

![](https://www.nextree.co.kr/content/images/2021/01/hjkwon-140324-prototype-07.png)

```js
function Person(name) { // 생성자 함수 정의
	this.name = name || "혁준"; // 매개변수가 없으면 "혁준"이 들어감
}

Person.prototype.getName = function() { // 인스턴스 메서드 선언 
	// prototype의 기능을 이용해 getName이라는 메서드를 정의
    return this.name;
};

function Korean(name) {}

Korean.prototype = new Person(); // 인스턴스 생성

var kor1 = new Korean();
console.log(kor1.getName());  // 혁준

var kor2 = new Korean("지수");
console.log(kor2.getName());  // 혁준
```

- 위 코드를 보면 부모에 해당하는 함수는 Person이다.

- 10라인에서 자식 함수인 Korean 함수 안의 [[prototype]] [[속성(Property)]]을 부모 함수로 생성된 [[객체(Object)]]로 바꿨다.

- 이제 Korean 함수와 [[new]] 연산자를 이용하여 생성된 kor [[객체(Object)]]의 `__proto__` [[속성(Property)]]이 부모 함수를 이용하여 생성된 [[객체(Object)]]를 참조한다.
- 이 [[객체(Object)]]가 Korean 함수를 이용하여 생성된 모든 객체의 프로토타입 객체가 된다. 

- kor1에는 name과 getName() 이라는 속성이 없지만, 부모에 해당하는 프로토타입객체에 name이 있다.

- 이 프로토타입객체의 부모에 getName()을 가지고 있어 kor1에서 사용할 수 있다. 

- 이 방법에도 단점이 있다.
- 부모 객체의 [[속성(Property)]]과 부모 객체의 프로토타입([[prototype]]) [[속성(Property)]]을 모두 물려받게 된다.

- 대부분의 경우 객체 자신의 [[속성(Property)]]은 특정 [[인스턴스(Instance)]]에 한정되어 재사용할 수 없어 필요가 없다.

- 또한, 자식 객체를 생성할 때 인자를 넘겨도 부모 객체를 생성할 때 인자를 넘겨주지 못한다. 

- 위의 코드 kor2 객체를 생성할 때 Korean 함수의 인자로 지수라고 주었다. 
- [[객체(Object)]]를 생성한 후 getName()을 호출하면 지수라고 출력될 거 같지만, 부모 [[생성자(Constructor)]]에 인자를 넘겨주지 않았기 때문에 name에는 default 값인 혁준이 들어 있다.

- [[객체(Object)]]를 생성할 때마다 부모의 [[함수(Function)]]를 호출할 수도 있다.
- 하지만 매우 비효율적이다. 
- 따라서 밑의 방법은 위의 방법의 문제점을 해결하는 방법이다.

## [[생성자(Constructor)]] 빌려 쓰기

- 이 방법은 기본 방법의 문제점인 자식 함수에서 받은 인자를 부모 [[함수(Function)]]로 인자를 전달하지 못했던 부분을 해결한다.
- 부모 함수의 [[this]]에 자식 객체를 바인딩하는 방식이다.

![](https://www.nextree.co.kr/content/images/2021/01/hjkwon-140324-prototype-08.png)



```js
function Person(name) {
    this.name = name || "혁준";
}

Person.prototype.getName = function(){ // Person 함수에 getName 메서드 정의
    return this.name;
};

function Korean(name){
    Person.apply(this, arguments);
}

var kor1 = new Korean("지수");
console.log(kor1.name);  // 지수
```

- 위의 코드를 보면 Korean [[함수(Function)]] 내부에서 [[apply()]] 함수를 이용한다.
- 부모 객체인 Person [[함수(Function)]] 영역의 this를 Korean [[함수(Function)]] 안의 [[this]]로 [[바인딩(binding)]]한다. 

- 이것은 부모의 [[속성(Property)]]을 자식 [[함수(Function)]] 안에 모두 복사한다.
- [[객체(Object)]]를 생성하고, name을 출력한다.

- [[객체(Object)]]를 생성할 때 넘겨준 인자를 출력하는 것을 볼 수 있다.

- 기본 방법에서는 부모 [[객체(Object)]]의 멤버를 참조를 통해 물려받았다.

- 하지만 [[생성자(Constructor)]] 빌려 쓰기는 부모 [[객체(Object)]] 멤버를 복사하여 자신의 것으로 만들어 버린다는 차이점이 있다.

- 하지만 이 방법은 부모 객체([[prototype]])의 this로 된 멤버들만 물려받게 되는 단점이 있다.

- 그래서 부모 [[객체(Object)]]의 프로토타입([[prototype]]) [[객체(Object)]]의 멤버들을 물려받지 못한다.
- 위의 코드 kor1 [[객체(Object)]]에서 부모 [[객체(Object)]]의 프로토타입([[prototype]]) [[객체(Object)]] 대한 링크가 없다는 것을 볼 수 있다.

## [[생성자(Constructor)]] 빌려 쓰고 프로토타입([[prototype]]) 지정해주기

- 이 방법은 위의 방법들의 문제점들을 보완하면서 자바에서 예상할 수 있는 동작 방식과 유사하다.

![](https://www.nextree.co.kr/content/images/2021/01/hjkwon-140324-prototype-09.png)


```js
function Person(name) {
    this.name = name || "혁준"; }

Person.prototype.getName = function(){
	return this.name;
};

function Korean(name){
    Person.apply(this, arguments);
}
Korean.prototype = new Person();

var kor1 = new Korean("지수");
console.log(kor1.getName());  // 지수
```

- 위킈 코드에서부모 함수 this를 자식 함수 this로 [[바인딩(binding)]]한다.
- 자식 함수 [[prototype]] [[속성(Property)]]을 부모 함수를 사용하여 생성된 객체로 지정했다.

- 부모 [[객체(Object)]] [[속성(Property)]]에 대한 참조를 가지는 것이 아닌 복사본을 통해 내 것으로 만든다.

- 동시에 부모 [[객체(Object)]]의 프로토타입([[prototype]]) [[객체(Object)]]에 대한 링크도 참조된다.

- 부모 객체의 프로토타입 [[객체(Object)]] 멤버도 사용할 수 있다. 

- 부모 객체의 프로토타입 객체도 링크로 연결된 것을 볼 수 있다.
- 이 방법에도 문제점이 있다.

- 부모 [[생성자(Constructor)]]를 두 번 호출한다.

- 생성자 빌려 쓰기 방법과 달리 getName()은 제대로 상속되었지만, name에 대해서는 kor1 객체와 부모 함수를 이용하여 생성한 객체에도 있는 것을 볼 수 있다.

## 프로토타입 공유

- 이번 방법은 부모 생성자를 한 번도 호출하지 않으면서 프로토타입 객체를 공유하는 방법입니다.

![](https://www.nextree.co.kr/content/images/2021/01/hjkwon-140324-prototype-10.png)

```js
function Person(name) {
    this.name = name || "혁준";
}

Person.prototype.getName = function(){
    return this.name;
};

function Korean(name){
    this.name = name;
}    
Korean.prototype = Person.prototype;

var kor1 = new Korean("지수");
console.log(kor1.getName());  // 지수
```

- 위 코드에서 자식 함수의 prototype 속성을 부모 함수의 prototype 속성이 참조하는 객체로 설정했다.

- 자식 함수를 통해 생성된 객체는 부모 함수를 통해 생성된 객체를 거치지 않고 부모 함수의 프로토타입 객체를 부모로 지정하여 객체를 생성한다. 

- 부모 함수의 내용을 상속받지 못하므로 상속받으려는 부분을 부모 함수의 프로토타입 객체에 작성해야 사용자가 원하는 결과를 얻게 된다. 

- 비교했을 때 중간에 부모 함수로 생성한 객체가 없고 부모 함수의 프로토타입 객체로 링크가 참조되는 것을 볼 수 있다.

## prototypal한 방식의 재사용

- 이 방법은 Object.create()를 사용하여 객체를 생성과 동시에 프로토타입 [[객체(Object)]]를 지정한다. 
- 이 함수는 첫 번째 [[매개변수(parameter)]]는 부모 객체로 사용할 객체를 넘겨주고, 두 번째 매개 변수는 선택적 매개변수로써 반환되는 자식 객체의 [[속성(Property)]]에 추가되는 부분이다.

- 이 함수를 사용함으로 써 객체 생성과 동시에 부모객체를 지정하여 코드의 재활용을 간단하게 구현할 수 있다.

```js
var person = {
    type : "인간",
    getType : function(){
        return this.type;
    },
    getName : function(){
        return this.name;
    }
};

var joon = Object.create(person); // create() 메서드를 이용하여 protoypal한 방식을 채택
joon.name = "혁준";

console.log(joon.getType());  // 인간
console.log(joon.getName());  // 혁준
```

- 위의 코드에서 부모 객체에 해당하는 person을 [[객체(Object)]] [[리터럴(literal)]] 방식으로 생성했다.

- 위의 코드에서 자식 객체 joon은 Object.create() 함수를 이용하여 첫 번째 매개변수로 person을 넘겨받아 joon 객체를 생성하였다.

- 한 줄로 객체를 생성함과 동시에 부모객체의 속성도 모두 물려받았다.
- classical 방식보다 간단하면서 여러 가지 상황을 생각할 필요도 없다.
- 자바스크립트에서 [[new]] 연산자와 함수를 통해 생성한 객체를 사용하는 classical 방식보다 prototypal 방식을 더 선호한다.