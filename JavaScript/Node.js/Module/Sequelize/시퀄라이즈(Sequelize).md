- [[노드(Node.js)]]에서 [[SQL]] 작업을 쉽게 할 수 있도록 도와주는 라이브러리([[모듈(Module)]])로 [[ORM(Object Relational Mapping)]]를 통해 여러 [[SQL]]류의 [[관계형 데이터베이스(Relational DataBase)]]와도 호환된다.

- 즉, 자바스크립트 문법으로도 [[데이터베이스(DataBase)]] 조작이 가능하다.

- 작은 프로젝트에서 사용하기는 매우 용이하다.

## 시퀄라이즈 설치

```bash
yarn add express morgan nunjucks sequelize sequelize-cli mysql2 
```

### 시퀄라이즈 구조 생성

- npx sequelize init 명령어를 통해 시퀄라이즈 구조를 생성한다.

- 이후 생성되는 models 디렉토리 안의 index.js에 내용을 아래와 같이 수정한다.
- mysql 기준 연결 버전이다.
 
```js
const Sequelize = require('sequelize');

const env = process.env.NODE_ENV || 'development';
const config = require('../config/config')[env];
const db = {};

// new Sequelize는 클래스 객체이기 때문에 인스턴스화를 여러개를 시킨다면 2개 이상의 mysql에 접속도 가능함
const sequelize = new Sequelize(config.database, config.username, config.password, config);
db.sequelize = sequelize;

module.exports = db;
```

- config/congin.json에 실제 development에 실제 mysql에 대한 정보를 연결한다.

```json
{
	"development": {
		"username": "root",
		"password": "Tmdcks6502@",
		"database": "nodejs",
		"host": "127.0.0.1",
		"dialect": "mysql"
	},
	...
}
```

- 데이터 베이스를 생성하는 명령어는 npx sequelize db:create이다.


## [[SQL]]과 시퀄라이즈 [[쿼리(Query)]] 변환

### [[CREATE]]

- 아래 [[SQL]]은 nodejs [[데이터베이스(DataBase)]]에 users [[테이블(Table)]]에 필드([[열(Column)]])에 값을 넣는 [[쿼리(Query)]]문이다.

```sql
INSERT INTO nodejs.users (name, age, married, comment) VALUES ('zero', 24, 0, '자기소개1'); 
```

- 위의 [[쿼리(Query)]]문을 시퀄라이즈 문법으로 변환하면 다음과 같다.
- 또한 create의 경우 [[Promise]] [[객체(Object)]]이기 때문에 [[async await]]을 붙여 사용해야 한다.

```js
const { User } = require("../models);

User.create({ // insert문과 같음
	name: 'zero',
	age: 24,
	married: false,
	comment: '자기소개1',
})
```

### [[SELECT]]

- 아래는 [[테이블(Table)]]의 모든 필드([[열(Column)]])을 선택하는 경우이다.

```sql
SELECT * FROM nodejs.users;
```

```js
const { User } = require("../models);

User.findAll({});
```

- 아래는 [[테이블(Table)]]의 특정 필드([[열(Column)]])을 선택하는 경우이다.

```sql
SELECT name, married FROM nodejs.users;
```

```js
const { User } = require("../models);

User.findAll({
	attributes: ['name', 'married'],
});
```

### [[WHERE]]

- 특수한 기능들인 경우 Sequelize.Op의 연산자를 이용한다.
- gt(greater than), lt(less than), gte(이상) lte(이하), ne(!=), in(안에 있은 데이터 확인), or 등이 있다.

- 기본적으로 같은 [[객체(Object)]]에 있으면 and 연산자로 묶인다.
- Op.or은 or 연산일때 Op.gt는 >일 때 사용한다.

- married가 0이면서 age가 30을 넘을 때이다.

```sql
SELECT name, age FROM nodejs.users WHERE married = 1 AND age > 30;
```

```js
const { Op } = require("sequelize");
const { User } = require("../models");

User.findAll({
	attributes: ['name', 'age'],
	where: {
		married: true,
		age: { [Op.gt]: 30 },
	}
});
```

- married가 0이거나 age가 30을 넘을 때이다.
- or의 경우 OP.or 연산자를 명시해준다.

```sql
SELECT id, age FROM nodejs.users WHERE married = 0 AND age > 30;
```

```js
const { Op } = require("sequelize");
const { User } = require("../models");

User.findAll({
	attributes: ['id', 'name'],
	where: {
		[Op.or]: [{ married: false }, { age: { [Op.gt]: 30} }],
	}
});
```

- age 내림차순으로 정렬하는 방법이다.

```sql
SELECT id, name FROM users ORDER BY age DESC;
```

```js
const { User } = require("../models");

User.findAll({
	attributes: ['id', 'name'],
	where: [['age', 'DESC']],
});
```

- age 내림차순으로 정렬하는데 한개의 LIMIT을 정의하는 예시이다.

```sql
SELECT id, name FROM users ORDER BY age DESC LIMIT 1;
```

```js
const { User } = require("../models");

User.findAll({
	attributes: ['id', 'name'],
	where: [['age', 'DESC']],
	limit: 1,
});
```

- [[OFFSET]]으로 기준점이 1로 설정하는 예시이다.

```sql
SELECT id, name FROM users ORDER BY age DESC LIMIT 1;
```

```js
const { User } = require("../models");

User.findAll({
	attributes: ['id', 'name'],
	where: [['age', 'DESC']],
	limit: 1,
});
```

### [[UPDATE]]

```sql
UPDATE nodejs.users SET comment = "바꿀 내용" WHERE id = 2;
```

```js
const { User } = require("../models");

User.update({
		comment: "바꿀 내용",
	}, 
	{
		where: { id: 2 },
	}
);
```

### [[DELETE]]

```sql
DELETE FROM nodejs.users WHERE id = 2;
```

```js
const { User } = require("../models");

User.destory({
		where: { id: 2 },
	}
);
```


## 시퀄라이즈를 통한 [[SQL/스키마(Schema)|스키마(Schema)]] 선언

- 아래는 models 디렉토리에 js파일을 생성하고 [[ORM(Object Relational Mapping)]]을 이용하여 
[[테이블(Table)]]을 만드는 방법이다.

- 예를 들어 user.js와 comment.js를 만든다고 가정하면 아래와 같다.

```js
const Sequelize = require('sequelize');

  
class User extends Sequelize.Model {
	static initiate(sequelize) {
		User.init(
			// init 메서드에 컬럼을 정의해서 넣을 수 있음
			{
			// 시퀄라이즈는 id를 자동으로 넣어주기 때문에 생략이 가능함
				name: {
					type: Sequelize.STRING(20), // VARCHAR가 아니라 STRING임
					allowNull: false,
					unique: true,
				},
				age: {
					type: Sequelize.INTEGER.UNSIGNED,
					allowNull: false,
				},
				married: {
					type: Sequelize.BOOLEAN, // 시퀄라이즈에서는 BOOLEAN mysql에서는 TIMYINT
					allowNull: false,
				},
				comment: {
					type: Sequelize.TEXT,
					allowNull: true,
				},
				created_at: {
					type: Sequelize.DATE, // DATETIME. MYSQL DATE -> Sequelize DateOnly
					allowNull: false,
					defaultValue: Sequelize.NOW,
				},
			},
			// 모델에 대한 설정
			{
				sequelize,
				timestamps: false, // createdAt, UpdateAt을 잠깐 끄기 위해 false로 설정
				underscored: false, // 시퀄라이즈의 글자들을 스네이크케이스(_)로 할지 카멜케이스로 할지 설정
				paranoid: true, // true이면 deletedAt까지 만들어줌 또한 소프트 delete를 가능하게 해줌
				modelName: 'User', // 모델이름(자바스크립트에서 쓰는 이름)이 단수라면
				tableName: 'users', // 테이블이름(sql에서 사용하는 이름) 은 복수형으로 만들어줌
				charset: 'utf8mb4',
				collate: 'utf8mb4_general_ci',
			}
		);
	}
	
	static associate(db) {
		db.User.hasMany(db.Comment, {
			foreignKey: 'commenter',
			sourceKey: 'id',
		});
	}
}


module.exports = User;
```

```js
const Sequelize = require('sequelize');

  
class Comment extends Sequelize.Model {
	static initiate(sequelize) {
		Comment.init(
			{
				comment: {
					type: Sequelize.STRING(100),
					allowNull: false,
				},
				created_at: {
					type: Sequelize.DATE,
					allowNull: true,
					defaultValue: Sequelize.NOW,
				},
			},
			{
				sequelize,
				timestamps: false,
				modelName: 'Comment',
				tableName: 'comments',
				paranoid: false,
				charset: 'utf8mb4',
				collate: 'utf8mb4_general_ci',
			}
		);
	}
	static associate(db) {
		db.Comment.belongsTo(db.User, {
			foreignKey: 'commenter',
			targetKey: 'id',
		});
	}
}
  

module.exports = Comment; 
```

- initiate [[메서드(Method)]]의 첫번째 [[매개변수(parameter)]]는 [[테이블(Table)]]의 필드([[열(Column)]])이고 두번째 [[매개변수(parameter)]]는 [[테이블(Table)]]의 설정이다.

- 다음은 models/index.js 설정이다.

```js
const Sequelize = require('sequelize');
const User = require('./user');
const Comment = require('./comment');

  
const env = process.env.NODE_ENV || 'development';
const config = require('../config/config')[env];
const db = {};

// new Sequelize는 클래스 객체이기 때문에 인스턴스화를 여러개를 시킨다면 2개 이상의 mysql에 접속도 가능함
const sequelize = new Sequelize(
	config.database,
	config.username,
	config.password,
	config
);

db.sequelize = sequelize;

  
db.User = User;
db.Comment = Comment;

  
User.initiate(sequelize);
Comment.initiate(sequelize);

  
User.associate(db);
Comment.associate(db);

  
module.exports = db;
```

