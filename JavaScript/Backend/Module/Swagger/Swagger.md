 - [[NestJS]]는 [[의존성 주입(Dependency Injection)]]이 명확하기 때문에 swagger를 사용하여 api 문서를 빠르게 생성할 수 있다.

## Swagger 설치 및 설정

- 아래 명령어를 통해 두개의 모듈을 설치한다.

```bash
 yarn add @nestjs/swagger swagger-ui-express
```

- main.ts 설정의 SwaggerModule.setup()을 설정한다.

```ts
import { NestFactory } from '@nestjs/core';  
import { AppModule } from './app.module';  
import { Logger } from '@nestjs/common';  
import { ConfigService } from '@nestjs/config';  
import { DocumentBuilder, SwaggerModule } from '@nestjs/swagger';  
  
declare const module: any;  

async function bootstrap() {  
    const configService = new ConfigService();  
    const logger = new Logger();  
	  
    const app = await NestFactory.create(AppModule);  
    const port = configService.get('port') || 3000;  
	  
    const config = new DocumentBuilder()  
        .setTitle('Sleact API') // api 페이지 이름
        .setDescription('Sleact 개발을 위한 API 문서입니다.') // api 페이지 설명
        .setVersion('1.0') // api 버전
        .addCookieAuth('connect.sid') // 쿠키 옵션
        .build();
		  
    const document = SwaggerModule.createDocument(app, config);  
    SwaggerModule.setup('api', app, document);  
	  
    await app.listen(port);  
	  
    logger.log(`listening on port ${port}`);  
	
    if (module.hot) {  
        module.hot.accept();  
        module.hot.dispose(() => app.close());  
    }  
}  
  
bootstrap();
```

- 설정을하게 되면 [[컨트롤러(Controller)]]의 [[핸들러(Handler)]]들을 자동으로 감지하여 api 문서를 만들게 된다.


## Swagger [[데코레이터(Decorator)]]

### [[컨트롤러(Controller)]]

- [[@ApiTag()]]를 사용하여 api를 그룹화할 수 있다.
  - [[@ApiOperation()]]를 사용하여 api에 대한 보충설명한다.
  - [[@ApiQuery()]]를 사용하여 [[쿼리스트링(Querystring)]]를 보충설명한다.  
  - [[@ApiParam()]]를 사용하여 [[URL 파라미터(URL Parameter)]]를 보충설명한다.
  - [[@ApiResponse()]]를 통해 응답 [[객체(Object)]]와 [[HTTP 응답 상태(status)]]를 정의할 수 있다.
### [[엔티티(Entity)]]

- [[@ApiProperty()]]를 사용하여 [[DTO(Data Transfer Object)]]의 [[속성(Property)]]들에 대한 보충설명을 하고 [[엔티티(Entity)]]에 붙이는 것이 이후 [[DTO(Data Transfer Object)]]를 서언할 때, 그래도 적용되어 편하다.
