- 모델은 [[JavaScript/스키마(schema)|스키마(schema)]]를 사용하여 만드는 [[인스턴스(Instance)]]로, [[데이터베이스(DataBase)]]에서 실제 작업을 처리할 수 있는 [[함수(Function)]]들을 지니고 있는 [[객체(Object)]]이다.
- 모델은 일반적으로 데이터를 표현하고 처리하는 일련의 규칙과 메서드를 정의하는 부분을 나타낸다.

- MVC(Model-View-Controller) 패턴에서 모델은 어플리케이션의 비즈니스 로직과 데이터를 담당한다.

## [[Node.js/Nest.js/NestJS]]의에서 모델

- 모델은 어플리케이션의 정보, 데이터를 나타낸다. 
- [[데이터베이스(DataBase)]], 초기화 된 상수나 값, 변수 등을 뜻한다. 
- 비즈니스 로직을 처리한 후 모델의 변경 사항을 [[컨트롤러(Controller)]]와 뷰에 전달한다.

### 모델 생성 규칙

- 사용자가 편집하길 원하는 모든 데이터를 가지고 있어야 한다.

- 뷰나 [[컨트롤러(Controller)]]에 대해서 어떤 정보도 알지 말아야 한다.
- 변경이 일어나면, 변경 통지에 대한 처리 방법을 구현해야만 한다.

### Class vs Interface

- [[클래스(class)]]를 이용하거나 인터페이스(Interface)를 이용하여 모델을 정의할 수 있다.

- 인터페이스(Interface)의 경우 [[변수(Variable)]]의 타입만을 체크한다.
- [[클래스(class)]]로 정의한 경우 [[변수(Variable)]]의 타입도 체크하고 [[인스턴스(Instance)]] 또한 생성할 수 있다.



## [[시퀄라이즈(Sequelize)]]에서 모델

- [[시퀄라이즈(Sequelize)]]에서 [[JavaScript/스키마(schema)|스키마(schema)]]를 통해 [[테이블(Table)]]을 만드는 문법은 다음과 같다.
- [[class]] [[키워드(Keyword)]]를 이용하여 [[클래스(Class)]] 선언을 통해 static [[메서드(Method)]]로 [[생성자(Constructor)]]와 [[연관 관계(Relationships)]]를 설정한다.

- [[생성자(Constructor)]]에서는 [[테이블(Table)]]에 필드([[열(Column)]])에 대한 설정을 한다.

```js
const Sequelize = require('sequelize');

class User extends Sequelize.Model {
	static initiate(sequelize) {
		// 필드에 대한 설정
	}
	
	// 연관 관계 설정
	static associate(db) {}
}


module.exports = User;
```

- 주의해야될 점은 모델을 잘못만들 었을 시에 코드만 바꾼다고 해서 필드([[열(Column)]])의 속성이 바뀌지는 않는다.
- 따라서 이 때는 [[DROP]]을 하고 다시 실행시키거나 특정 명령어로 필드를 수정해야 한다.