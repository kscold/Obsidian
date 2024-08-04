 - 자바스크립트에서는 에러 처리가 매우 중요하다.
- 언어 특성상 싱글쓰레드이기 때문에 [[에러(error)]]가 발생하면 복구가 매우 어렵다.

- 또한 [[노드(Node.js)]]에서는 에러 처리를 제대로 하지 않으면 서버가 죽어버리는 현상까지 발생하기 때문에 에러를 처리해야 한다.

- [[throw]] 원하는 [[에러(error)]]만 처리할 수도 있다.


## [[try catch]]로 Error 객체 처리

- try문 안에 그 코드를 넣고, catch문 안에 에러가 났을 때 복구할 방법을 적는다.

```js
try {
  // 에러가 나는 코드
} catch (e) {
  console.error(e);
}
```

- 이제 본격적으로 Error 객체에 관해 알아보자.
- 아래 console.dir는 객체를 콘솔에 로깅하는 [[메서드(Method)]]이다.

```js
try {
	a.b.c.d. = f; // Uncaught SyntaxError: Unexpected token =
} catch (e) {
	console.dir(e);
}
```

- 먼저 문법 오류이다.
- d 뒤에 점이 붙어 있어 에러가 난다.
- 이 경우는 catch문에 에러가 걸리지 않고 바로 try 문에서부터 에러가 난다. 
- 문법 오류는 내서는 안 된다는 뜻이다.

```js
try {
	a.b.c.d = f;
} catch (e) {
	console.dir(e); // ReferenceError: a is not defined
}
```

- 위의 코드는 이제 문법은 유효하지만, 코드를 실행하는 과정에서 에러가 발생한다.
- console.dir를 통해 에러인 e가 객체임을 확인할 수 있다.
- e는 다음과 같이 생겼다.

```jsx
{
  message: 'a is not defined',
  stack: 'ReferenceError: a is not defined\n    at <anonymous>:2:3',
  __proto__: Error
}
```

- [[prototype]]이 Error인 객체이다.
- message에는 에러 메시지가, stack에는 에러가 코드의 어떤 부분에서 발생했는지 나와 있다. 

- [[prototype]](`__proto__`)를 살펴보면 다음과 같다.

```jsx
{
  constructor: ReferenceError()
  message: '',
  name: 'ReferenceError',
  toString: toString(),
  __proto__: Object,
}
```

프로토타입에 에러의 이름이 들어있습니다. 이 이름을 통해서 에러가 어떤 종류의 에러인지 알 수 있겠죠?

SyntaxError, ReferenceError 외에도 자바스크립트에는 TypeError(공포의 cannot read property 'x' of undefined 에러가 이 유형입니다), RangeError, EvalError, URIError 등이 있습니다.

자, 이제 직접 에러를 만들어봅시다. 직접 에러를 만드는 경우가 언제인지 궁금하시죠? 문법이나 코드 상에서는 어떠한 에러도 없지만, 의미적으로 이 상황에서 에러가 나야한다고 할 때 만들어줍니다. 예를 들면 로그인 시 가입한 아이디가 없는 경우(단순히 결과가 없다는 것은 에러가 아니지만, 로그인 로직 아래에서는 의미상 에러가 될 수 있습니다)가 있습니다.

로그인 로직 자체를 코드로 예시를 들어보겠습니다.

```jsx
function findUser(id, password) {
  User.findById(id, function(err, user) { // 아이디로 회원 찾기
    if (err) {
      throw err;
    }
    if (!user) {
      return '아이디에 해당하는 회원이 없습니다';
    }
    if (user.password === password) {
      return '로그인 성공';
    } else {
      return '비밀번호 틀림';
    }
  });
}
```

여기서 에러가 하나 나오는데요 바로 err 부분입니다. 회원 정보에 대한 DB 요청을 하는 부분(findById)에서 에러가 날 수 있는데 이 부분은 저희가 어떻게 할 수 있는 부분이 아니니까 넘어갑니다. 하지만 이 부분도 DB 요청 라이브러리에서 커스텀 에러로 만들어두었을 확률이 높습니다.

저희는 회원이 없거나 비밀번호가 틀렸을 경우에 집중합시다. 코드 상에서는 에러가 아니지만 비밀번호가 틀렸기 때문에 에러로 볼 수 있습니다. return 부분을 throw로 바꿔서 실행해봅시다. new Error로 새로운 에러를 만들고 첫 번째 전달인자로 메시지를 넣어주면 됩니다.

```jsx
function findUser(id, password) {
  User.findById(id, function(err, user) { // 아이디로 회원 찾기
    if (err) {
      throw err;
    }
    if (!user) {
      throw new Error('아이디에 해당하는 회원이 없습니다');
    }
    if (user.password === password) {
      return '로그인 성공';
    } else {
      throw new Error('비밀번호 틀림');
    }
  });
} 
```

이 함수를 실행하는 입장에서 에러를 처리해볼까요?

```jsx
var User = {
  findById: function(id, cb) {
    if (id === 'zerocho') {
      cb(null, { password: '1234' });
    } else {
      cb(null, null);
    }
  },
};
```

일단 User랑 findById를 모킹(테스트를 간단히 하기 위해서 복잡한 객체를 간단한 객체로 대체하는 것)해주고요.

다음 두 가지 경우를 테스트해보겠습니다.

```jsx
try {
  findUser('zerocho', '2345');
} catch (e) {
  console.dir(e); // Error: 비밀번호 틀림
}
```

```jsx
try {
  findUser('nero', '1234');
} catch (e) {
  console.dir(e); // Error: 아이디에 해당하는 회원이 없습니다.
}
```

위와 같이 두 경우 모두 에러가 잘 발생하지만, 둘 다 기본 에러라서 구별이 쉽지 않습니다. 특히 catch 문 안에서 비밀번호가 틀린 경우와 아이디에 해당하는 회원이 없을 경우 두 가지를 한 번에 처리하고 싶을 때 문제가 생깁니다.

```jsx
try {
  findUser('zerocho', '1234');
} catch (e) {
  if (비밀번호틀린에러) {
    // 비밀번호틀린에러 처리
  } else if (회원없음에러) {
    // 회원없음에러 처리
  }
}
```

catch문 안에서 저 if들을 어떻게 구분해야 할까요? 각각 다른 에러 이름을 붙여주면 좋을 것 같습니다.

```jsx
function findUser(id, password) {
  User.findById(id, function(err, user) { // 아이디로 회원 찾기
    if (err) {
      throw err;
    }
    if (!user) {
      var err = new Error('아이디에 해당하는 회원이 없습니다');
      err.name = 'NoUserError';
      throw err;
    }
    if (user.password === password) {
      return '로그인 성공';
    } else {
      var err = new Error('비밀번호 틀림');
      err.name = 'WrongPasswordError';
      throw err;
    }
  });
}
```

이런 식으로 에러 객체에 name 속성을 붙여주는 방법이 있을 수 있습니다.

```jsx
try {
  findUser('zerocho', '2345');
} catch (e) {
  if (e.name === 'NoUserError') {
    console.log('회원이 없을 때 에러를 처리');
  } else if (e.name === 'WrongPasswordError') {
    console.log('비밀번호틀렸을 때 에러를 처리');
  } else {
    console.log('누구냐 넌');
  }
}
```

이제 name으로 구별이 가능합니다. 

하지만 한 가지 아쉬운 점이 있다면 `console.error(e)`의 에러 메시지는 그대로 Error: 비밀번호 틀림입니다. WrongPasswordError: 비밀번호 틀림이 되게 할 수는 없을까요? 이 때는 ES2015 문법이 필요합니다. 만약 ES5 문법으로도 나오게 하려면 `console.error(e.stack)`을 하면 됩니다. ES2015 문법으로 하면 쓸데 없이 복잡해지는게 그렇게 해야 될 효용을 느끼지는 못하겠네요.

이상으로 Error 객체에 관한 강좌를 마칩니다. 커스텀 에러를 만들어서 자유자재로 분기처리 해보세요.

