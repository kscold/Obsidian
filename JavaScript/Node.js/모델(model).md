- 모델은 [[JavaScript/스키마(schema)|스키마(schema)]]를 사용하여 만드는 [[인스턴스(Instance)]]로, [[데이터베이스(DataBase)]]에서 실제 작업을 처리할 수 있는 [[함수(Function)]]들을 지니고 있는 [[객체(Object)]]이다.


## [[시퀄라이즈(Sequelize)]]에서 모델 선언 문법

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