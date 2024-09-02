

  


### Controller-level Pipe

컨트롤러 레벨에서 `@UsePipes()` 데코레이터를 이용하면 현재 컨트롤러에 정의되어 있는 모든 **핸들러에 `pipe`가 적용된다.**

```ts
@Controller('cats')
@UsePipes(ValidationPipe)
export class CatsController {
  //...
}
```

  

### Handler-level Pipe

핸들러 레벨에서 `@UesPipes()` 데코레이터를 이용하여 사용할 수 있다. 핸들러 레벨에서 `pipe`를 사용하게 될 경우 핸들러의 **모든 파라미터에 적용이 된다.**

```ts
@Post()
@UsePipes(ValidationPipe)
saveCatInfo(@Body() req: CreateCatDto) {
    //...
}
```

  

### parameter-level pipe

파라미터 레벨의 `pipe`이기 대문에 **특정한 파라미테에만 적용이 되는**`pipe`이다.

```ts
@Get('/:id')
findById(@Param('id', ParseIntPipe) id: number) {
    //...
}
```

  

### Global-level pipe

애플리케이션이 `bootstrap`되는 `main.ts`에서 설정이 가능하다.

```ts
@filename(main.ts)
async function bootstrap() {
  const app = await NestFactory.create(AppModule);
  app.useGlobalPipes(new ValidationPipe())
  await app.listen(3000);
}
bootstrap();
```

위와 같이 `app.useGlobalPipes`에 `ValidationPipe`를 생성하여 인스턴스를 넣어주면 된다.

하지만 위의 `code`의 경우 **의존성 주입**관점에서 `main.ts`에 직접 선언해주기 때문에 `code`가 유연하지 않다. 추 후 `GlobalPipes`가 변경될 경우 `main.ts`를 수정해주어야 한다.

`NestJs`에서는 위와 같은 방법 말고 `root Module`에서 `Custom Provider`를 이용하여 `GlobalPipes`를 등록할 수 있는 방법을 제공해준다.

```ts
@filename(app.module.ts)
@Module({
  imports: [CatsModule],
  controllers: [],
  providers: [
    {
      provide: APP_PIPE,
      useClass: ValidationPipe
    }
  ],
})
export class AppModule { }
```

이렇게 `CustomProvider`로 정의를 해주게 되면 `GlobalPipes`로 사용할 `pipe`가 변경되더라도 유연하게 대채할 수 있게 된다.