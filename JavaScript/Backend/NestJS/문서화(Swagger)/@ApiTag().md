- [[NestJS]]의 [[Swagger]]를 작성할 때, [[컨트롤러(Controller)]] [[데코레이터(Decorator)]] 선언 레벨에 작성하여 설명으로 섹션을 나눠 그룹화할 수 있는 [[데코레이터(Decorator)]]이다.


## 예시

- 아래 코드처럼 작성하면 DmsController에 대한 api 정의는 [[Swagger]]에서 DM으로 나뉜다.

```ts
@ApiTags('DM')  
@Controller('api/workspaces/:url/dms')  
export class DmsController {  

}
```