- 객체란, 현실의 사물을 코딩을 통해 컴퓨터에게 설명하기 위해 만든 덩어리이다.
- 자바스크립트에서는 기본 데이터 타입인 boolean, number, string, 그리고 특별한 값인 null, [[undefined]] 빼고는 모두 객체이다.
- 따라서 [[함수(Function)]]도 객체이며 이 때문에 [[일급 함수(First Class Function)]]와 [[일급 객체(First Class Object)]]의 특징을 가진다.

- { }를 사용해서 [[객체(Object)]]를 만드는 것이 더 일반적이며 이를 객체 [[리터럴(literal)]]이라고 한다.

- 객체 하나만 사용하는 것이 아니라 [[배열(Array)]]을 통한 또 하나의 덩어리를 통해 접근하여 사용하기도 한다.

## 객체의 예시

- 객체의 key와 value의 형태의 딕셔너리를 [[속성(Property)]]이라고 한다.

```jsx
var zero = {
  firstName: 'Zero',
  lastName: 'Cho'
};
```

- 따라서 zero 객체에는 `firstName: 'Zero'`와 `lastName: 'Cho'`까지 두 개의 속성이 있다.
- 속성끼리는 쉼표로 구분해준다. 
- [[클래스(class)]] 내부에 정의 된 [[함수(Function)]]를 [[메서드(Method)]]라고 부르는 것 뿐만 아니라 [[속성(Property)]]값이 [[함수(Function)]]인 것을 우리는 [[메서드(Method)]]라고 부른다.

