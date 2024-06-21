- [[시퀄라이즈(Sequelize)]]의 indclude [[속성(Property)]]을 통해 [[JOIN]]과 비슷한 기능을 수행할 수 있다.


## 예시

- 결과값이 자바스크립트 [[객체(Object)]]로 반환된다.

```js
const user = await User.findOne({});
console.log(user.nick); // 사용자 닉네임
```

- 아래예시처럼 [[연관 관계(Relationships)]]을 엮을 수 있다.

```js
const user = await User.findOne({
	include: [{
		model: Comment,
	}]
});
console.log(user.Comment) ; // 사용자 댓글
```

- [[다대다(ManyToMany)]] [[모델(model)]]은 다음과 같이 접근이 가능하다.

```js
db.sequelize.models.PostHashtag
```


## [[WHERE]]나 attributes([[열(Column)]])

- include나 관계 [[쿼리(Query)]] [[메서드(Method)]]에도 [[WHERE]]나 attributes를 사용할 수 있다.

- 댓글의 id가 1인 데이터를 가져오는 2가지 방법이다.
- 2번째 방법의 경우 [[get+모델명]]의 방식을 사용한 것이다.

```js
const user = await User.findOne({
	include: [{
		model: Comment,
		where: {
			id: 1,
		},
		attributes: ['id'],
	}]
});

// 또는 

const comments = await User.getComments({
	where: {
		id: 1,
	},
	attributes: ['id'],
});
```