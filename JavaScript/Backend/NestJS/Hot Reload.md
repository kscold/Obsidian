- 개발을 할 때 코드를 작성하고, 새로 작성한 코드를 적용하기 위해서는 서버를 멈추고 재시작을 반복해야 한다.
- [[NestJS]]는  nodemon [[모듈(Module)]] 이외에도 Hot Reload 기능을 추가해서 사용할 수 있다.

## Hot Reload 설치

- 아래 명령어로 필요한 패키지들을 설치한다.

```
npm i --save-dev webpack-node-externals run-script-webpack-plugin webpack
```

- 패키지들을 설치 한 후 프로젝트 폴더에 webpack-hmr.config.js 파일을 생성한 후 아래 코드를 작성한다.

```js
const webpack = require('webpack');  
const nodeExternals = require('webpack-node-externals');  
const { RunScriptWebpackPlugin } = require('run-script-webpack-plugin');  
  
module.exports = function (options) {  
    return {  
        ...options,  
        entry: ['webpack/hot/poll?100', options.entry],  
        watch: true,  
        externals: [  
            nodeExternals({  
                allowlist: ['webpack/hot/poll?100'],  
            }),  
        ],  
        plugins: [  
            ...options.plugins,  
            new webpack.HotModuleReplacementPlugin(),  
            new webpack.WatchIgnorePlugin({
                paths: [/\.js$/, /\.d\.ts$/], // 배열 대신 객체의 paths 속성을 사용해야 함  
            }),  
            new RunScriptWebpackPlugin({ name: options.output.filename }),  
        ],  
    };  
};
```

- HMR(Hot-Module-Replacement)를 활성화하기 위해 main.ts 파일에 웹팩 관련 내용을 추가한다.
- [[declare]]를 사용하여 module를 [[import]]한다.

```ts
import { NestFactory } from '@nestjs/core';  
import { AppModule } from './app.module';  
import { Logger } from '@nestjs/common';  
  
declare const module: any;
  
async function bootstrap() {  
    const logger = new Logger();  
    const app = await NestFactory.create(AppModule);  
    const port = process.env.PORT || 3000;  
  
    await app.listen(port);  
  
    logger.log(`listening on port ${port}`);  
  
    if (module.hot) {  
        module.hot.accept();  
        module.hot.dispose(() => app.close());  
    }  
}  
  
bootstrap();
```

- 실행을 간단하게 하기 위해서 package.json 파일에서 스크립트를 수정해준다.

```json
"scripts" : {
	"start:dev": "nest build --webpack --webpackPath webpack-hmr.config.js --watch"
}
```

- 이제 아래와 같이 실행할 수 있다.

```bash
npm run start:dev
yarn start:dev
```