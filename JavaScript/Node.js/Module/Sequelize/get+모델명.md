- [[시퀄라이즈(Sequelize)]]에서 get+[[모델(model)]]명의 형식으로 [[연관 관계(Relationships)]]에 있는 데이터를 로딩가능하다.


## 예시

```js
const user = await User.findOne({});
const comments = await user.getComments(); // get+모델명으로 데이터를 불러옴

console.log(comments); // 사용자 댓글
```


## as : [[모델(model)]]

- as 모델명으로 변경도 가능하다.
- 모호하게 만들기 때문에 자주 사용되는 문법은 아니다.

```js
// 관계
db.User.hasMany(db.Comment, { foreignKey: 'commenter', sourceKey: 'id', as: 'Answers'});

const user = await User.findOne({});
const comments = await user.getAnswers();
console.log(comments); // 사용자 댓글
```