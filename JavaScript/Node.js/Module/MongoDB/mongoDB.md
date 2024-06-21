
.remove(): 특정 조건을 만독하는 데이터를 모두 지운다.
.findByIdAndRemove(): id를 찾아서 지운다.
.findOneAndRemove() : 특정 조건을 만족하는 데이터 하나를 찾아서 제거한다.

 
 Query.prototype.findOneAndDelete()

 
 이 함수는 findOneAndRemove() 명령이 MongoDB findAndModify() 명령이 된다는 점에서 Model.findOneAndRemove ()와 약간 다릅니다. 대부분의 mongoose 사용 사례에서 이 구별은 순전히 현학적입니다. 타당한 이유가 없다면 findOneAndDelete()를 사용해야 합니다.

## 형식
 
```js
Model.findByIdAndDelete(id, options, callback);
```
