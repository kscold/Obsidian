- PartialType은 [[NestJS]]의 `@nestjs/swagger` 패키지에서 제공하는 유틸리티로, 기존 [[DTO(Data Transfer Object)]]를 기반으로 모든 속성을 선택적([[@IsOptional()]])으로 만드는 역할을 한다.

- 따라서 PartialType은 기존 [[DTO(Data Transfer Object)]]를 확장하여 새로운 [[DTO(Data Transfer Object)]]를 생성하므로, 코드 중복을 줄일 수 있다.
- 원본 [[DTO(Data Transfer Object)]]의 모든 필드에 `@IsOptional()`을 적용하여, 업데이트 요청 시 모든 속성을 선택적으로 사용할 수 있게 한다.