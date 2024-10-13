- class-validation은 타입의 안정성을 높이고 원하는 값이 입력되는 가를 검증하는 데에 사용되는 라이브러리이다.

- 기존의 자바스크립트의 문법으로 원하는 값을 얻으려고 하면 각 validate 메소드를 작성을 하여야 했으나, 간단한 [[데코레이터(Decorator)]] 추가 함으로 인해 쉽게 검증을 할 수 있게 되었다.

- 비슷한 라이브러리로 [[class-transformer]]가 있다.
- [[NestJS]]에서 필수적으로 사용하는 라이브러리 중 하나이다.



## class-validator의 종류
### 공통          

 [[@IsDefined()]]
 [[@IsOptional()]]
 [[@Equals()]]
 [[@NotEquals()]]  
 [[@IsEmpty()]]    
 [[@IsNotEmpty()]]
 [[@Isin()]]
 @IsNotin 
### 타입

 [[@IsBoolean()]]
 [[@IsDateString()]]
 [[@IsString()]]
 [[@IsNumber()]]
 [[@IsInt()]]
 [[@IsArray()]]
 [[@IsEnum()]]
## 숫자

[[@IsDivisibleBy()]]
[[@isPositive()]]
[[@IsNegative()]]
[[@Min()]]
[[@Max()]]

### 문자

@Contains
@NotContains
@IsAlphanumeric
@IsCreditCard
@IsHexColor
[[@MaxLength()]]
[[@MinLength()]]
@IsUUID
@IsLatLng