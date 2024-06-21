- 직접 [[시퀄라이즈(Sequelize)]]에서 [[쿼리(Query)]]문을 사용할 수 있게 만들어주는 [[메서드(Method)]]이다.


## 예시

- 아래 예시처럼 직접 [[SQL]]문법을 사용할 수 있다.₩

```js
const [result, metadata] = await sequelize.query('SELECT * FROM comments');
console.log(result);
```