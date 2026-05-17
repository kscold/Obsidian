- [[NestJS]]에서는 [[데이터베이스(DataBase)]] Layer와 View Layer 사이의 역할을 분리하기 위해 [[엔티티(Entity)]]랑 [[DTO(Data Transfer Object)]]를 분리해서 사용한다.

- 이 [[DTO(Data Transfer Object)]]를 원하는 return값으로 뽑기위해서 매핑을 위해 plainToInstance를 사용한다.


## 문법

- plainToInstance는 [[plain type]]의 [[객체(Object)]]를 [[class type]]으로 매핑해준다.

```ts
const some = plainToInstance(classType, plainType, option);
```

## [[class type]]

- 변경될 형식이다.
- 주로 [[DTO(Data Transfer Object)]]가 들어간다.

## [[plain type]]

- 변경시킬 형식이다.

### excludeExtraneuosValues

- 옵션의 한 종류이다.
- 이 옵션의 true는 [[@Expose()]] 밖에서 정의되지 않은 옵션 제거, [[클래스(class)]]가 가지고 있지 않은 [[속성(Property)]]들은 제외시키는 옵션이다.

- 우선 plainToInstance부터 살펴보면 헛점이 많다고 생각된다.
- plainToInstance는 plain type의 [[객체(Object)]]를 class type로 Mapping한다.
- 허나, 이 class가 모든 [[속성(Property)]]를 보장해주진 못한다.