
```js
A.findByIdAndUpdate(id, update, options, callback) // executes
A.findByIdAndUpdate(id, update, options)  // returns Query
A.findByIdAndUpdate(id, update, callback) // executes
A.findByIdAndUpdate(id, update)           // returns Query
A.findByIdAndUpdate()                     // returns Query

Model.findByIdAndUpdate(id, {name: 'jason bourne'}, options, callback)
Model.findByIdAndUpdate(id, {$set: {name: 'jason bourne'}}, options, callback)
```

파라미터

Model.findByIdAndUpdate(id, update, options, callback)

- `id`: 업데이트할 문서를 식별하기 위한 고유한 식별자 (일반적으로 문서의 `_id` 필드).
- `update`: 업데이트할 내용을 담은 객체.
- `options` (선택사항): 업데이트 옵션을 설정하는 객체. 예를 들어, `{ new: true }`를 사용하면 업데이트된 문서가 반환된다.
- `callback` (선택사항): 비동기 작업이 완료될 때 실행할 콜백 함수.

option
- new : 수정된 Document는 true이고 초기 default 값은 false
- upsert : 존재하지 않았던 것을 생성해 업데이트 하면 true, 초기 default false
- runValidators : 모델 스키마에 반하는 업데이타가 일어날 때 유효화 작업을 진행하면 true
- setDefaultsOnInsert : default 값은 true 이고 upsert도 true이면 새로운 document가 생성되면 default 값을 적용함
- sort : 조건에 맞는 많은 docs를 찾을 때 정렬 방법을 정함
- select : document를 return으로 세팅함
- rawResult : true이면 mongoDB에서 raw 한 결과를 return 함
- strict : 업데이트를 통해 스키마의 strict mode로 덮어씀