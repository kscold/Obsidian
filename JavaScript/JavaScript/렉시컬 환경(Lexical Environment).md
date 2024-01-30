- `Lexical Environment`는 코드 `block`, `function`, `script`를 실행하기 앞서 생성되는 특별한 [[객체(Object)]]로, 실행할 [[스코프(Scope)]] 범위 안에 있는 [[변수(Variable)]]와 [[함수(Function)]]를 [[속성(Property)]]으로 저장하는 객체다.

- 즉 우리가 소스 코드를 실행하면서 참조가 필요한 변수의 값을 이 Lexical Environment 라는 객체에서 [[식별자(Identifier)]] 이름을 키로 찾는다고 보면 된다.

## [[실행 컨텍스트(execution context)]]와의 관계

### Lexical Enviroment 구조

렉시컬 환경은 2가지 부분으로 이뤄져 있다.

1. `Environment Record`  
    `Environment Record`(이하 환경 레코드)는 모든 로컬 변수들을 프로퍼티로 갖는 객체다. (그리고 this같은 특별한 식별자도 포함할 수 있다.)
2. 외부 코드와 연관된 외부 렉시컬 환경에 대한 참조

### Lexical Enviroment의 작동 방식

먼저 전체 스크립트와 관련이 있는 렉시컬 환경은 **글로벌** 렉시컬 환경(Global Lexical Enviroment)이다. 전역 코드가 **시작되면 렉시컬 환경 객체가 만들어지고, 코드를 실행하기 앞서** 선언되어 있는 변수와 함수를 먼저 글로벌 환경 레코드에 저장한다. 이 때, 저장 방식이 변수 또는 함수 선언 방식에 따라 조금 차이가 있다.

- 변수가 var로 선언된 경우  
    var로 선언된 변수는 환경 레코드에 변수 이름을 key로, `undefined`를 value로 하여 초기화된다. 위에서도 말했지만 이것은 코드를 한줄 한줄 실행시키기 전에 일어나는 과정이므로, 우리가 흔히 **호이스팅**이라고 하여 변수 선언이 끌어올려진 것처럼 보인다고 하는 현상도 이 때문이다. 분명 변수가 선언되기 전에 참조했음에도 에러가 나지 않고 undefined 라는 값을 얻을 수 있기 때문이다.
    
- 변수가 let이나 const로 선언된 경우  
    let, const로 선언된 경우에는 환경 레코드에 변수 이름을 key로, `<uninitialized>` 라는 상태를 value로 초기화한다. undefined와 달리 이 상태는 참조할 수 없는 값이다. 그래서 이 변수가 선언된 줄에 도달하기 전에 변수를 참조하고자 한다면, var와 달리 `ReferenceError`가 발생한다. 그래서 변수 선언 줄에 도달하기까지의 영역을 변수에 접근할 수 없다는 의미로 `Temporary Dead Zone`이라고 하기도 한다.위 코드에서 func 함수 바깥에 x가 선언되어 있음에도 레퍼런스 에러가 난다. func 함수 안에 있는 console.log(x) 가 실행될 때 로컬 변수 x는 분명히 존재하지만 선언된 줄에 이를 때까지는 참조할 수 없는 TMZ에 있다. 그래서 원래 로컬에서 찾을 수 없으면 외부 렉시컬 환경을 찾아봐야 하는데, 로컬에 x가 분명히 있기는 하지만 참조할 수 없으므로 외부의 렉시컬 환경을 참조하는 대신 레퍼런스 에러를 발생시킨다.
    
    ```javascript
    let x = 1; function func() { console.log(x); // the local variable x is known to the engine from the beginning of the function, // but "uninitialized" (unusable) until let ("dead zone") // hence the error console.log(x); // ReferenceError: Cannot access 'x' before initialization let x = 2; } func();
    ```
    
- 함수가 선언식으로 선언된 경우var로 선언된 변수와 달리 함수 선언식은 함수 이름을 key로 하고 함수 자체를 value로 저장하여 **완전하게 초기화**된다. var로 선언된 변수는 선언 줄 전에 접근하면 초기화 값인 undefined를 리턴하는 반면, 함수 선언식은 초기화될 때 완전하게 저장되므로 선언 줄 이전에 접근하더라도 사용할 수 있다.
    
- `console.log(sum(1,2)); // 3 function sum(a,b) { return a + b; }`
    
- 함수가 표현식으로 선언된 경우변수에 익명 함수를 저장하는 형식인 표현식은, 변수 선언자에 따라 다르다.
    
    ```javascript
    console.log(sum(1,2)); // Uncaught TypeError: sum is not a function var sum = function(a,b) { return a + b; }
    ```
    
    ```javascript
    console.log(sum); // undefined var sum = function(a,b) { return a + b; }
    ```
    
- `console.log(sum); // Uncaught ReferenceError: sum is not defined const sum = function(a,b) { return a + b; }`
    

### 외부 렉시컬 환경

위에서 렉시컬 환경은 2가지 파트로 구성돼 있고 그 중 하나가 외부 렉시컬 환경에 대한 참조라고 했다.

코드를 실행할 때 필요한 변수를 해당 로컬 스코프와 연관돼 있는 환경 레코드에서 먼저 찾고, 그 안에 없으면 렉시컬 환경이 갖고 있는 외부 렉시컬 환경에 대한 참조로 접근해서 그 외부 환경 레코드에 있는지 또 찾는다. 이런 식으로 글로벌까지 타고 올라가서 식별자를 검색하고 없으면 에러가 발생하는 것이다. (참고로 에러가 나는 건 use strict 모드에서만 그렇다고 한다. 아닌 경우에는 새로운 글로벌 변수를 만들어낸다는 듯...)

### 클로저

이제 렉시컬 환경이 어떻게 클로저를 가능하게 하는지 살펴보자.

```javascript
function makeCounter() {  let count = 0;   return function() {    return count++;  };} let counter = makeCounter();
```

**makeCounter라는 함수의 호출이 시작될 때, 새로운 렉시컬 환경이 만들어진다.** 그리고 makeCounter 함수 실행에 필요한 변수를 저장할 것이다. 거기에는 count 라는 로컬 변수가 저장될 것이다.

그리고 여기서 특이한 점은 함수를 리턴하고 있는데, 그 함수가 리턴하는 것이 `count++` 라는 것이다. 어쨌거나 makeCounter 함수가 실행하면서 저 리턴 함수는 **만들어져서** counter 변수에 저장될 것이다. 포인트는 counter 함수가 만들어지기만 했다는 것이다. 이 함수를 실행하지는 않았다.

이제 counter 변수에는 함수가 만들어진 함수가 저장돼 있고, counter를 호출하면, 0이라는 값이 리턴된다.

즉 makeCounter에 있었던 count 변수를 참조할 수 있다는 뜻이다.

이것이 어떻게 가능하냐면, **모든 함수는 `[[Environment]]` 라는 내부 프로퍼티를 갖고 있다.** **이 프로퍼티는 함수가 만들어질 때 그 함수를 둘러싼 외부 렉시컬 환경에 대한 참조를 저장한다.** 다시 한번 언급하지만 실행될 때가 아니고 함수가 만들어질 때다. 위 예시로 들자면 makeCounter가 실행되면 counter 함수는 만들어지기만 한 상태이다. 이 때 이미 외부 렉시컬 환경 즉 makeCounter의 렉시컬 환경에 대한 참조를 `counter.[[Enviroment]]` 프로퍼티에 저장한 것이다. 그리고 counter 함수가 나중에 호출될 때, 이 때 비로소 counter 함수의 렉시컬 환경 객체가 생성되고, 이 객체가 외부 렉시컬 환경에 대한 참조를 `counter.[[Enviroment]]` 프로퍼티로부터 가져온다. 그렇게 해서 이 counter함수가 언제 어디서 실행되든 이미 만들어질 때 makeCounter의 렉시컬 환경에 대한 참조를 저장했으므로 count 변수를 참조할 수 있다. 그리고 이런 것을 `클로저`라고 한다.

정리해서 말하자면 **클로저란 외부 변수를 기억하고 접근할 수 있는 함수를 말한다.** 그리고 자바스크립트에서는 사실 모든 함수가 클로저인 셈이다.

### 가비지 콜렉션

makeCounter이 여러 번 호출돼서 counter1, counter2, counter3 ... 이런 식으로 여러 개의 카운터가 생기더라도 이들이 리턴하는 값은 독립적이다. 왜냐하면 makeCounter가 실행될 때마다 렉시컬 환경이 새롭게 만들어지고, 카운터 함수는 그 새롭게 만들어진 (즉 count가 0인 상태) 렉시컬 환경을 참조할 것이기 때문이다. 이렇게 되면 makeCounter와 관련된 렉시컬 환경이 여러 개가 생기고, 각 counter1, counter2, counter3 ... 가 각각 참조를 갖고 있는 상태이고, 참조가 사라져서 unreachable 하게 될 때 비로소 makeCounter와 관련된 렉시컬 환경 객체는 메모리에서 clean된다.