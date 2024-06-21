- 원래 모던 자바스크립트 이전에는 [[클래스(Class)]]라는 개념이 없었다.
- 프로토타입 [[객체(Object)]]는 새로운 [[객체(Object)]]가 생성되기 위한 원형이 되는 객체이다.
- 같은 원형으로 생성된 객체가 공통으로 참조하는 공간이다.

- 따라서 원래 자바스크립트는 기존의 [[객체(Object)]]를 복사하여(cloning) 새로운 [[객체(Object)]]를 생성하는 프로토타입 기반의 언어이다.

- 프로토타입 기반 언어는 객체 원형인 프로토타입을 이용하여 새로운 객체를 만들어낸다.([[DOM(Document Object Model)]] 구조이다.)

- 프로토타입 객체의 멤버를 읽는 경우에는 객체 또는 함수의 prototype 속성을 통해 접근할 수 있다. 
- 하지만 추가, 수정, 삭제는 함수의 prototype 속성을 통해 접근해야 한다.
- 이렇게 생성된 객체 역시 또 다른 객체의 원형이 될 수 있다.

- 프로토타입은 객체를 확장하고 객체 지향적인 프로그래밍을 할 수 있게 해준다.

- 프로토타입은 크게 두 가지로  객체를 참조하는 prototype 속성과 객체 멤버인 proto이 참조하는 숨은 링크로 해석된다.

## 함수와 [[객체(Object)]]의 구조와 과정

### 1. 함수와 객체의 내부 구조

- 자바스크립트에서는 함수를 정의하고, 파싱([[파싱(Parsing)]])단계에 들어가면, 내부적으로 수행되는 작업이 있다.
- [[함수(Function)]] 멤버로 prototype [[속성(Property)]]이 있다. 
- 이 속성은 다른 곳에 생성된 함수이름의 프로토타입 [[객체(Object)]]를 참조한다. 
- 프로토타입 객체의 멤버인 [[생성자(Constructor)]] 속성은 함수를 참조하는 내부구조를 가진다. 

![[Pasted image 20231015173220.png]]

```js
function Person()(}
```

- 속성이 하나도 없는 Person이라는 함수가 정의되고, 파싱단계에 들어가면, Person 함수 Prototype 속성은 프로토타입 객체를 참조한다.
- 프로토타입 객체 멤버인 constructor 속성은 Person 함수를 참조하는 구조를 가진다.
- 여기서 알아야 하는 부분은 Person 함수의 prototype 속성이 참조하는 프로토타입 객체는 [[new]]라는 연산자와 Person 함수를 통해 생성된 모든 객체의 원형이 되는 객체이다.
- 생성된 모든 객체가 참조한다는 것을 기억해야 한다.

![[Pasted image 20240119013246.png]]

```js
function Person(){}

var joon = new Person();
var jisoo = new Person();
```


- 자바스크립트에서 기본 데이터 타입인 boolean, number, string, 그리고 특별한 값인 null, [[undefined]] 빼고는 모두 [[객체(Object)]]이다.
- 사용자가 정의한 [[함수(Function)]]도 [[객체(Object)]]이고, [[new]]라는 연산자를 통해 생성된 것도 [[객체(Object)]]이다.
- 객체 안에는 proto(비표준) [[속성(Property)]]이 있다.
- 이 속성은 객체가 만들어지기 위해 사용된 원형인 프로토타입 객체를 숨은 링크로 참조하는 역할을 한다.

### 2. 프로토타입 객체란?

- [[함수(Function)]]를 정의하면 다른 곳에 생성되는 프로토타입 객체는 자신이 다른 객체의 원형이 되는 객체이다.
- 모든 객체는 프로토타입 객체에 접근할 수 있다.
- 프로토타입 객체도 동적으로 런타임에 멤버를 추가할 수 있다. 
- 같은 원형을 복사로 생성된 모든 객체는 추가된 멤버를 사용할 수 있다.

![](https://www.nextree.co.kr/content/images/2021/01/hjkwon-140324-prototype-03.png)

```js
function Person(){}

var joon = new Person();
var jisoo = new Person();

Person.prototype.getType = function (){ 
    return "인간"; 
};

console.log(joon.getType());   // 인간
console.log(jisoo.getType());  // 인간
```

- 함수 안의 prototype 속성을 이용하여 멤버를 추가했다.
- 프로토타입 객체에 getType()이라는 함수를 추가하면 멤버를 추가하기 전에 생성된 객체에서도 추가된 멤버를 사용할 수 있다.
- 같은 프로토타입을 이용하여 생성된 joon과 jisoo 객체는 getType()을 사용할 수 있다.

- 여기서 알아두어야 할 것은 프로토타입 객체에 멤버를 추가, 수정, 삭제할 때는 함수 안의 prototype 속성을 사용해야 한다.
- 하지만 프로토타입 멤버를 읽을 때는 함수 안의 prototype 속성 또는 객체 이름으로 접근합니다.

![](https://www.nextree.co.kr/content/images/2021/01/hjkwon-140324-prototype-04.png)


```js
joon.getType = function (){ 
    return "사람"; 
};

console.log(joon.getType());   // 사람
console.log(jisoo.getType());  // 인간

jisoo.age = 25;

console.log(joon.age);   // undefined
console.log(jisoo.age);  // 25
```

- joon 객체를 이용하여 getType() 리턴 값을 사람으로 수정하였다.
- 그리고 joon과 jisoo에서 각각 getType()을 호출하면 joon 객체를 이용하여 호출한 결과는 사람으로 출력되고, jisoo로 호출한 결과는 인간이 출력된다.
- 생성된 객체를 이용하여 프로토타입 객체의 멤버를 수정하면 프로토타입 객체에 있는 멤버를 수정하는 것이 아니라 자신의 객체에 멤버를 추가하는 것이다. 
- joon 객체를 사용하여 getType()을 호출하면 프로토타입 객체의 getType()을 호출한 것이 아니다.
- joon 객체에 추가된 getType()을 호출한 것이다.
- 프로토타입 객체의 멤버를 수정할 경우는 멤버 추가와 같이 함수의 prototype 속성을 이용하여 수정한다.

![](https://www.nextree.co.kr/content/images/2021/01/hjkwon-140324-prototype-05.png)

```jsx
Person.prototype.getType = function (){
    return "사람"; 
};

console.log(jisoo.getType());  // 사람
```

- 함수의 prototype 속성을 이용하여 getType() 리턴 값을 사람으로 수정한다. 
- 그리고 jisoo 객체를 이용하여 호출한 결과 사람이 나온다.
