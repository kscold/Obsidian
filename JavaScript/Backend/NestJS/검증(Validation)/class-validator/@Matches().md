- [[class-validator]]에서 [[정규표현식(Regex)]]을 사용하할 수 있는 [[데코레이터(Decorator)]]이다.


## 문법

```ts
@Matches(정규표현식, {message : ""}) 
```

- 첫번째 [[매개변수(parameter)]]로는 [[정규표현식(Regex)]]을 입력한다.
- 두번째 [[매개변수(parameter)]]로는 [[정규표현식(Regex)]]에 맞지 않았을 때 [[에러(error)]] 메세지를 반환한다.

## 예시

- 따라서 아래와 같이 사용할 수 있다.
```ts
@Matches(RegExp('^[가-힣a-zA-Z0-9]*$'), {message : "입력 값을 다시 확인하세요"}) 
```