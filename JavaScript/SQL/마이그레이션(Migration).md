- 마이그레이션(Migration)은 [[데이터베이스(DataBase)]] 변경사항을 스크립트로 작성해서 반영한다.
- 통제된 사오항에서 [[데이터베이스(DataBase)]] [[스키마(Schema)]] 변경 및 복구를 진행할 수 있다.


## 마이그레이션(Migration)이 필요한 이유

### Controlled Changes

- 원하는 상황에 원하는 형태로 마이그레이션을 자유롭게 실행할 수 있다.
### Reversible

- 진행한 마이그레이션을 쉽게 되돌릴 수 있다.
### Versioning

- 마이그레이션은 [[스키마(Schema)]] 변경에 대한 히스토리를 담고 있다.
- 즉, 디버깅에 배우 유용하다.
### Consistency

- 다양한 환경에서 [[데이터베이스(DataBase)]] [[스키마(Schema)]]가 길게 유지도되도록 할 수 있다.
### Complex Changes

- 복잡한 [[데이터베이스(DataBase)]]의 변화를 직접 컨트롤할 수 있다.