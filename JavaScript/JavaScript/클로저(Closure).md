- 클로저는 [[함수(Function)]]와 [[함수(Function)]]가 선언된 어휘적 환경([[렉시컬 환경(Lexical Environment)]])의 조합이다.

- 즉, [[함수(Function)]] 안에 함수를 선언한 환경에서의 관계를 의미한다.

- 해당 함수안에 함수를 선언한 환경은 내부 함수에서 외부 함수로 지역변수를 접근할 수 있지만 외부 함수의 실행이 끝나고 외부 함수가 소멸된 이후에도 내부 함수가 외부 함수의 변수에 접근할 수 있는 것을 의미한다.

- 이러한 이유가 발생하는 이유는 자바스크립트는 [[함수(Function)]]를 반환하고 반환되는 [[함수(Function)]]가 클로저를 형성하기 때문에 접근이 가능하다.
- 클로저는 반환된 내부함수가 자신이 선언되었을때의 환경([[렉시컬 환경(Lexical Environment)]])에서의 [[스코프(Scope)]]를 기억하기 때문에 접근이 가능하다.

- 정리해서 말하자면 클로저란 외부 [[변수(Variable)]]를 기억하고 접근할 수 있는 [[함수(Function)]]를 말한다.
- 그리고 자바스크립트에서는 사실 모든 [[함수(Function)]]가 클로저인 셈이다.


## 클로저가 가능한 이유

- 이제 렉시컬 환경이 어떻게 클로저를 가능하게 하는지 살펴보자.

```js
function makeCounter() {
	let count = 0;   
	return function() {    
		return count++;  
	};
}

let counter = makeCounter();
```

- makeCounter라는 함수의 호출이 시작될 때, 새로운 [[렉시컬 환경(Lexical Environment)]]이 만들어진다.

- 그리고 makeCounter 함수 실행에 필요한 변수를 저장할 것이다. 
- 거기에는 count 라는 로컬 변수가 저장될 것이다.

- 그리고 여기서 특이한 점은 [[함수(Function)]]를 리턴하고 있는데, 그 함수가 리턴하는 것이 `count++` 라는 것이다. 

- 어쨌거나 makeCounter 함수가 실행하면서 저 리턴 함수는 만들어져서 counter 변수에 저장될 것이다. 
- 포인트는 counter 함수가 만들어지기만 했다는 것이다. 
- 이 함수를 실행하지는 않았다.

- 이제 counter 변수에는 함수가 만들어진 함수가 저장돼 있고, counter를 호출하면, 0이라는 값이 리턴된다.

- 즉 makeCounter에 있었던 count 변수를 참조할 수 있다는 뜻이다.

- 이것이 어떻게 가능하냐면, 모든 [[함수(Function)]]는 `[[Environment]]` 라는 내부 [[속성(Property)]]를 갖고 있다.

- 이 [[속성(Property)]]는 [[함수(Function)]]가 만들어질 때 그 함수를 둘러싼 [[렉시컬 환경(Lexical Environment)]]에 대한 참조를 저장한다.

- 다시 한번 언급하지만 실행될 때가 아니고 함수가 만들어질 때다.
- 위 예시로 들자면 makeCounter가 실행되면 counter 함수는 만들어지기만 한 상태이다. 
- 이 때 이미 외부 [[렉시컬 환경(Lexical Environment)]] 즉, makeCounter의 렉시컬 환경에 대한 참조를 `counter.[[Enviroment]]` 프로퍼티에 저장한 것이다.

- 그리고 counter [[함수(Function)]]가 나중에 호출될 때, 이 때 비로소 counter 함수의 렉시컬 환경 객체가 생성되고, 이 객체가 외부 렉시컬 환경에 대한 참조를 `counter.[[Enviroment]]` [[속성(Property)]]으로부터 가져온다.
- 그렇게 해서 이 counter함수가 언제 어디서 실행되든 이미 만들어질 때 makeCounter의 렉시컬 환경에 대한 참조를 저장했으므로 count 변수를 참조할 수 있다.

- 그리고 이런 것을 클로저라고 한다.
