- [[Iterator]] [[인터페이스(Interface)]]의 [[메서드(Method)]]로 자주 사용된다.
- boolean 타입만 반환가능하다.

### hasNext()와 [[next()]]의 차이
- 결정적으로 다른 점은 반환 타입이다.
- hasNext()는 boolean 타입으로 반환된다.
- 즉 "True or False"로 반환된다.
- 다음에 가져올 값이 있으면 True, 없으면 False이다.

- 하지만 next()는 "매개변수 혹은 iterator 되는 타입"으로 반환된다
- 즉 아무 타입으로 반환할 수 있다. 
- Iterator에 입력된 값들이 String이면 String 값으로 가져오는 것이다. 

- 만약에 1,2,3,4,5 라는 숫자들이 저장되어 있는 배열을 Iterator 인터페이스로 가지고 올 때, hasNext()는 "True or False"를 나타내겠지만, next()는 "숫자 값"들을 가지고 온다.


