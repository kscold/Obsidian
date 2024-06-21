- [[노드(Node.js)]]는 대부분 [[이벤트(event)]]를 기반으로 하는 [[비동기(asynchronous)]] 방식으로 처리한다.
- 그리고 [[비동기(asynchronous)]] 방식으로 처리하기 위해서 서로 다른 [[이벤트(event)]]를 전달한다.

- 예를 들어, 어떤 [[함수(Function)]]를 실행한 결과물도 [[이벤트(event)]]로 전달한다. 
- [[이벤트(event)]]는 한쪽에서 다른 쪽으로 알림 메세지를 보내는 것과 비슷하다. 

- 즉, '지금 이쪽의 상태는 이렇다' 는 정보를 다른 쪽으로 보내는 것이다.

- [[노드(Node.js)]]에서는 이런 이벤트를 보낼 수 있도록 EventEmitter 라는 것이 만들어져 있다.
- EventEmitter 는 events [[모듈(Module)]] 안에 정의되어 있다.


## EventEmitter의 동작 구조

- 밑의 이미지는 이벤트 에밋(EventEmitter)과 [[이벤트 리스너(Event Listener)]]를 통해서 [[이벤트(event)]]를 주고 받는 방법이다.

![](https://blog.kakaocdn.net/dn/u5OKj/btqOVKITCdX/TxeUNzLEc2Xmp5xgnwihPK/img.png)

## 이벤트 보내고 받기

- 노드의 객체는 EventEmitter를 상속받을 수 있으며, 상속받은 후에 EventEmitter [[객체(Object)]]의 [[on()]]과 emit() [[메서드(Method)]]를 사용할 수 있다.

### addListener() [[매개변수(parameter)]] ('이벤트 이름', [[콜백 함수(Callback Function)]])

- on() 메서드와 똑같이 설정된 [[이벤트(event)]] 이름에 [[콜백 함수(Callback Function)]]을 실행하는 형태로 사용한다.
### on() [[매개변수(parameter)]] ('이벤트 이름', [[콜백 함수(Callback Function)]])

- 메서드는, [[이벤트(event)]]가 전달된 [[객체(Object)]] [[이벤트 리스너(Event Listener)]]를 설정하는 역할을 한다.
- 이 리스너 함수는 객체로 전달된 이벤트를 받아서 처리할 수 있다. 

- 보통은 노드 내부에서 미리 만들어 제공하는 이벤트를 받아 처리하지만, 필요할 때는 직접 이벤트를 만들어 전달할 수 있다.

- on() 메서드 외에 once() 메소드를 사용할 수 있다.
### once() [[매개변수(parameter)]] ('이벤트 이름', [[콜백 함수(Callback Function)]])

- [[메서드(Method)]]를 사용할 때는 [[이벤트 리스너(Event Listener)]] 함수가 한 번이라도 실행하고 나면 자동으로 제거되므로 이벤트를 딱 한 번만 받아서 처리할 수 있다.
### emit() [[매개변수(parameter)]]('이벤트 이름')

- [[이벤트(event)]]를 실질적으로 호출하는 [[메서드(Method)]]이다.
- 이벤트를 다른 쪽으로 전달하고 싶다면 emit() [[메서드(Method)]]를 사용한다.

## removeListener() [[매개변수(parameter)]]('이벤트 이름')

- 지정한 [[이벤트(event)]]에 대한 리스너를 제거한다.


## [[on()]]을 사용하는 예시

```js
process.on('exit', function() {
	console.log('exit 이벤트 발생함.');
});

setTimeout(function() {
	console.log('2초 후에 시스템 종료 시도함.');
	process.exit();
}, 2000);

// >> 2초 후에 시스템 종료 시도함.  
// >> exit 이벤트 발생함.
```

- [[process]] [[객체(Object)]]는 [[노드(Node.js)]]에서 언제든지 사용할 수 있는 [[객체(Object)]]인데, 이미 내부적으로 EventEmitter를 [[상속(Inheritance)]]하도록 만들어져 있어서 [[on()]]과 emit() [[메서드(Method)]]를 바로 사용할 수 있다.

- process 객체의 [[on()]] [[메서드(Method)]]를 호출하면서 이벤트 이름을 exit 로 지정하면 프로세스가 끝날 때를 알 수 있다.

- 그 아래 코드는 [[setTimeout()]] [[메서드(Method)]]를 호출하여 2초 후 프로그램을 끝낸다.
- 이를 실행하면 콘솔 창에 결과가 출력된다.

- 그렇다면, 미리 정의되어 있는 이벤트가 아니라 우리가 직접 만든 이벤트는 어떻게 처리할 수 있을까?

- 위의 입력한 것과 비슷한 방식으로 아래 코드를 입력한다.

```js
process.on('tick', function(count) {
	console.log('tick 이벤트 발생함 : %s',count);
});

setTimeout(function() {
	console.log('2초 후에 tick 이벤트 전달 시도함.');
	process.emit('tick','2');
}, 2000);

// >> 2초 후에 tick 이벤트 전달 시도함.
// >> tick 이벤트 발생함 : 2
```

- tick 이벤트를 직접 만들고 2초 후에 [[setTimeout()]] [[메서드(Method)]]를 사용해 process.emit() [[메서드(Method)]]를 호출하면서 tick 이벤트를 process 객체로 전달했다.

- process.on() 메소드를 호출하여 [[이벤트(event)]]를 등록하면 이 메소드를 호출하면서 [[매개변수(parameter)]]로 전달한 tick 이벤트가 발생했을 때 그 다음에 나오는 [[콜백 함수(Callback Function)]]가 실행된다.


## 계산기 객체를 모듈로 만들어 보기

- 지금까지 실행해 본 코드만으로 한쪽에서 이벤트를 만들어 전달할 수 있고, 다른 쪽에서 그것을 받아서 처리할 수 있다.

- 그런데 항상 process 객체를 사용해 이벤트를 전달한다면 같은 이름의 이벤트를 사용하는 경우에 충돌이 발생할 수 있다.

- 이 때문에 별도의 모듈 파일을 만들고 그 안에서 이벤트를 처리하도록 만드는 것이 좋다.
- 그러면 앞에서 다룬 모듈 구성 방법을 잘 생각하면서 새로운 모듈을 만들어본다.

```js
// calc3.js
var util = require('util');
var EventEmitter = require('events').EventEmitter;

var Calc = function(){
	var self = this;
	
	this.on('stop', function(){
		console.log('Calc에 stop event 전달됨.');
	});
};

util.inherits(Calc, EventEmitter); // Calc 함수를 EventEmitter에 상속함

Calc.prototype.add = function(a, b){ // add 함수를 오버라이딩
	return a + b;
}

module.exports = Calc;
module.exports.title = 'calculator'; // title 속성 값으로 calculator로 설정
```


- [[require()]] [[메서드(Method)]]를 호출하여 events [[모듈(Module)]]을 불러들인 후 그 안에 [[속성(Property)]]으로 들어 있는 EventEmitter [[객체(Object)]]를 참조한다.

- Calc 객체는 계산기 객체로서 [[function]] [[키워드(Keyword)]]를 사용해 프로토타입([[prototype]]) [[객체(Object)]]로 만든다.
- 프로토타입 객체 안에서는 [[this]] [[키워드(Keyword)]]를 이용해 자기 자신을 가리킬 수 있으며, 그 [[객체(Object)]] 안에 정의된 [[속성(Property)]]에 접근할 수 있다. 

- 그런 다음 Calc 객체가 이벤트 처리를 할 수 있도록 EventEmitter를 [[상속(Inheritance)]]하도록 만든다. 
- 상속은 util 모듈에 있는 inherits() 메소드를 사용하면 쉽게 정의할 수 있다.

- 코드의 가장 위쪽에서 require() [[메서드(Method)]]를 호출하여 util 모듈을 불러왔기 때문에 util.inherits() [[메서드(Method)]]를 호출하는 코드를 넣어도 오류는 발생하지 않는다.

- Calc 객체 안에 들어있는 [[prototype]] 객체의 속성으로 add 함수를 추가하면 [[new]] 연산자를 이용해 Calc 객체의 [[인스턴스(Instance)]] [[객체(Object)]]를 만들었을 때, add()함수를 사용할 수 있다.

- 코드의 마지막 부분에는 calc3.js 파일에 정의한 모듈을 불러들이는 쪽에서 Calc 객체를 참조할 수 있도록 module.exports에 Calc 객체를 지정한다.
- 추가적으로 title 속성 값으로 calculator 라는 이름을 설정한다.

- 이 계산기 객체로 전달되는 stop 이벤트를 처리하기 위해서 Calc 객체 안에서 [[on()]] [[메서드(Method)]]를 호출하는 부분도 눈여겨볼 수 있다.

- 이렇게 만든 [[모듈(Module)]]을 사용하는 코드는 다음과 같이 별도의 파일에 입력한다.

```js
var Calc = require('./calc3');
var calc = new Calc();

calc.emit('stop'); // calc3.js 내부의 Calc의 stop 이벤트를 실행시킴

console.log(Calc.title + '에 stop 이벤트 전달함.');

// >> Calc에 stop event 전달됨.  
// >> calculator에 stop 이벤트 전달함.
```

- [[require()]] [[메서드(Method)]]를 호출하면서 `./calc3` 을 [[매개변수(parameter)]] 전달하면 별도의 모듈 파일에 정의해둔 Calc [[객체(Object)]]를 [[변수(Variable)]]로 사용할 수 있다.

- Calc 객체는 프로토타입([[prototype]]) 객체로 계산기 기능을 정의한 것이므로 먼저 [[new]] 연산자를 이용해 인스턴스 객체를 만든다.
- Calc 객체가 EventEmitter를 [[상속(Inheritance)]]하므로 [[인스턴스(Instance)]] 객체의 emit() [[메서드(Method)]]를 호출하여 stop 이벤트를 전달한다.

- stop 이벤트가 정상적으로 전달된 것을 볼 수 있다.