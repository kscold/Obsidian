- ES5까지  [[var]] [[키워드(Keyword)]]로만 유일하게 변수를 선언할 수 있었다.
- var 키워드의 함수는 재[[할당(assignmet)]]를 할 수 있다.
### var의 단점
- var 키워드의 여러 담점 중에서 가장 대표적인 것이 블록 레벨 스코프(block-level-scope)를 지원하지 않고 함수 레벨 스코프(function-level-scope)를 지원한다는 것이다.
- 이로 인해 의도치 않게 전역 변수가 선언되어 심각한 부작용이 발생하기도 한다.
### var
```js
var score;
```

- 위의 형태로 선언을 하면 score는 예시) 0x000000F2라는 주소의 [[undefined]](원시 값을) 가진다.