- 하나의 [[테이블(Table)]]을 써도 되지만 너무 많은 [[열(Column)]]들의 데이터가 있어 로딩이 너무 오래걸리는 경우에 이런 [[테이블(Table)]]을 두개로 쪼개어일대일 관계를 사용한다.

- 따라서 자주 사용되는 정보들만 뽑아 [[테이블(Table)]]에 넣어놓고 덜 사용되는 정보는 일대일 관계로 따로 넣어놓는다.


## [[시퀄라이즈(Sequelize)]]에서 사용

- 예시로 사용자 [[테이블(Table)]]과 사용자 정보 [[테이블(Table)]]을 들 수 있다.

![[Pasted image 20240413031623.png]]

```js
db.User.hasOne(db.Info, { foreignKey: 'UserId', sourceKey: 'id'});

db.Info.belongTo(db.User, { foreignKey: 'UserId', targetKey: 'id'});
```

- 위의 경우 일대일 관계이기 때문에 어느 [[테이블(Table)]]이 hasOne()이 되고 어느 [[테이블(Table)]]이 belongsTo()가 되는지는 임의적으로 정해주어야 한다.