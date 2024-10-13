- @IsOptional()를 이용하면 [[undefined]]를 받을 수 있으면서 값이 존재할 때는 [[@IsString()]], @IsNumber() 등으로 타입 체크도 가능하다.
- 따라서 @IsOptional() [[데코레이터(Decorator)]] 자체로는 큰 의미를 가지고 있지는 않다.


## 예시

- 아래 코드처럼 들어 값이 있어야지만 수정이 가능할 때 즉, POST로 특정 데이터가 등록이 되고, 이 데이터가 있을 때에만 수정이 가능할 때에는 @IsOptional() [[데코레이터(Decorator)]]와 [[@IsNotEmpty()]]를 사용하여 데이터가 있을 때에만 수정할 수 있도록 만들 수 있다.

```ts
import { IsNotEmpty, IsOptional } from 'class-validator';  
  
export class UpdateMovieDto {  
    @IsNotEmpty()  
    @IsOptional() // 값이 있는 경우에만 수정가능  
    title?: string;  
	
    @IsNotEmpty()  
    @IsOptional()  
    genre?: string;  
}
```
