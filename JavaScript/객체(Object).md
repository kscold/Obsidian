
- 객체란, 현실의 사물을 코딩을 통해 컴퓨터에게 설명하기 위해 만든 덩어리이다.
- 객체 하나만 사용하는 것이 아니라 [[배열(Array)]]을 통한 또 하나의 덩어리를 통해 접근하여 사용하기도 한다.

```jsx
var zero = {
  firstName: 'Zero',
  lastName: 'Cho'
};
```

[[속성(Property)]]

객체 안을 보면 firstName과 lastName이 왼쪽에 있고, 'Zero'와 'Cho'가 오른쪽에 있다. 콤마로 구분되는 것들을 객체의 **속성**이라고 부른다. zero 객체에는 `firstName: 'Zero'`와 `lastName: 'Cho'`까지 두 개의 **속성**이 있는거죠. 속성끼리는 쉼표로 구분해준다. 자바스크립트의 객체 속성은 key:value 형태의 딕셔너리로 구성되어 있음.

속성값이 함수인 것을 우리는 [[메서드(Method)]]라고 부름

### 객체 속성값에 접근하는 2가지 방법
- .를 찍고 접근하는 Dot Notation이 있다.
- `[]` 대괄호를 치고 접근하는 Bracket Notation이 있다.
- Bracket Notation보다는 주로 Dot Notation를 많이 사용한다.

```jsx
zero.firstName; // 'Zero' 
zero['firstName']; // 'Zero' 

zero.lastName; // 'Cho'
zero['lastName']; // 'Cho'
```

객체 속성 변경

```jsx
zero.lastName = 'Lee';
zero; // { firstName: 'Zero', lastName: 'Lee' }
```

- 객체 속성 추가 및 변경

```jsx
var zero = {
  body: {
    height: 173,
    weight: 66
  }
};
zero.body.height; // 173
```

### 객체의 key와 value 값을 이용한 접근

```jsx
const name = 'variantKey'
const object = {
	[name]: 'value' // variantKey : value로 설정된다.
};
```

### delete를 이용한 객체 삭제

```jsx
delete zero.body.height;
zero.body; // { weight: 66 }
```

### [[new]]를 이용한 객체 복사 생성

```jsx
var zero = new Object();
zero.firstName = 'Zero';
zero.lastName = 'Cho';
zero.body = new Object();
zero.body.height = 173;
zero.body.weight = 66;
```

{ }를 사용해서 객체를 만드는 것이 더 일반적이며 이를  객체 [[리터럴(literal)]]이라고 한다.


객체 중에는 특수한 객체가 있습니다. 바로 [[함수(Function)]]와 [[배열(Array)]]입니다.