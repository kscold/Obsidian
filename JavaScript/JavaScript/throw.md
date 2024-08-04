- throw는 간단하게 말하면 에러를 생성하는 [[키워드(Keyword)]]이다.

## 문법

```js
throw <error object>
```

- 이론적으로는 모든 자료형을 에러 객체([[원시 타입(Primitive type)]]도 포함)로 사용할 수 있다. 

- 하지만 이전 게시글에서 배웠던 기본 내장 에러(name, message를 [[속성(Property)]]으로 갖는 [[Error 객체]])와의 호환을 위해 name, message [[속성(Property)]]를 포함하는 객체로 에러 객체를 만들 것을 권장한다.

- 자바스크립트는 Error, SyntaxError, ReferenceError 등 우리가 흔히 보는 에러 이름으로 [[객체(Object)]] [[생성자(Constructor)]]를 지원한다. 

```js
let error = new Error(message);
let error = new SyntaxError(message);
let error = new ReferenceError(message);
```

- 일반 객체가 아닌 내장 생성자를 사용해 만든 내장 [[Error 객체]]의 `name` [[속성(Property)]]은 [[생성자(Constructor)]] 이름과 동일한 값을 갖는다.
- SyntaxError로 만들었으면 SyntaxError가 name [[속성(Property)]]이다.

- [[속성(Property)]] `message`의 값은 인자에서 가져온다.  

```js
let error = new Error('에러 발생~!');
alert(error.name); // Error
alert(error.message); // 에러 발생~1
```


## 서버에서 오류가 나는 예시

- 서버에서 사용자 정보를 가져온다고 가정한다.
- 사용자 정보에서 name은 포함되어 있겠다고 예상할 수 있다.  
- 그런데 만약 받아온 사용자 정보에 name이 없다면 아래 코드처럼 에러를 발생시켜서 잡아야한다.  

```js
try {
	let user = JSON.parse(json); // 서버에서 사용자 정보를 가져오는 코드
    	
    if(!user.name) {
      	throw new SyntaxError('불완전한 데이터: 이름 없음'); // 에러 생성
    }
} catch(e) {
  	alert('JSON Error: ' + e.message); // JSON Error: 불완전한 데이터: 이름 없음
}
```

- 그런데 위의 코드는 try에서 name을 받아오지 못한 에러로 catch에서 경고하게 된다.

- 그런데 만약 try에서 다른 에러가 발생한다면 의도치 않은 내용이 `e.message`에 담기게 된다.
- JSON Error가 아닌데도 말이다.

- 이 때 throw를 이용하면 된다.

- catch는 알고있는 에러만 처리하고 나머지는 다시 던진다.
- 이게 throw의 정의이다.

- 과정은 다음과 같다.  
1. `catch`는 모든 에러를 받는다  
2. `catch(err) {...}` 블럭 안에서 에러 객체 `err`을 분석한다.  
3. 에러 처리 방법을 알지 못하면 `throw err`를 한다.

- 이때 에러 타입 체크를 [[instanceof]] 명령어를 사용한다. 
- 그러나 현재 이 명령어는 지양해야한다고 한다.

```js
try {
  	user = {...}; // 변수 선언 키워드(let 등)를 사용하지 않았다. 에러 발생
} catch (err) {
	if(err instanceof ReferenceError) {
    	alert('ReferenceError');
    }
}
```

- 원래의 예시 돌아가서 throw를 적용해보면 다음과 같다.

```js
let json = '{ "age": 30 }'; // 불완전한 데이터
try {
	let user = JSON.parse(json);
	
	if (!user.name) {
	    throw new SyntaxError("불완전한 데이터: 이름 없음");
	}
	
	blabla(); // 예상치 못한 에러
	
	alert( user.name );
	
} catch(e) {
	if (e instanceof SyntaxError) {
	    alert( "JSON Error: " + e.message );
	} else {
		throw e; // 에러 다시 던지기 (*)
	}
}
```

- catch 안의 ()에서 다시 던져진 에러는 try catch 의 밖으로 던져진다.
- 이때 밖에 try catch 가 있다면 여기서 에러를 잡으며, 아니라면 스크립트는 죽는다.

- 이런 방식으로 catch 블럭에서는 어떻게 다룰지 알고 있는 에러만 처리하고, 알 수 없는 에러는 건너뛸 수 있다.