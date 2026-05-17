- 소스코드 안에서 어떠란 코드들은 개발 환경이나 운영 환경에 이러한 환경에 따라 다른게 코드를 넣어줘야 할 때가 있고, 남들에게 노출 되지 않아야 하는 코드들도 있기 때문에 이러한 코드들을 위해 설정 파일을 따로 만들어서 보관한다.

- 설정 파일은 [[런타임(runtime)]] 도중에 바뀌는 것이 아닌 어플리케이션이 시작할 때 로드가 되어서 그 값들을 정의하여 준다.
- 그리고 설정 파일은 여러가지 파일 형식(XML, [[JSON(Java Script Object Notation)]], YAML 등)을 사용할 수 있다.


## Codebase VS Enviroment Variables(환경 변수)
### Codebase

- XML, [[JSON(Java Script Object Notation)]], YAML 같은 경우는 Codebase에 해당한다.
- 일반적으로 Port 같이 노출되어도 상관 없는 정보들에 사용한다.
### 환경변수

- 비밀 번호나 API key같은 노출되면 안되는 정보들에 사용한다.


## Config 모듈을 이용한 설정 파일 생성

1. 루트 디렉토리에 config라는 폴더를 만든 후에 그 폴더 안에 [[JSON(Java Script Object Notation)]]이나 YAML 형식의 파일을 생성한다.

2. config 폴더 안에 default.yml, development.yml, 그리고 production.yml 파일을 생성한다.

### default.yml

- 개발 환경 설정이나 운영 환경 설정에도 똑같이 적용되는 기본 설정 파일이다.
### development.yml

- default.yml에서 설정한 것과 개발 환경에서 필요한 정보를 설정한 파일이다.

### production.yml

- default.yml에서 설정한 것과 운영 환경에서 필요한 정보를 설정한 파일이다.


## Config 모듈 사용

- 아래 명령어도 일단 모듈을 설치한다.

```bash
yarn add config
```

- 이후 사용하고 싶은 파일에서 import를 통해 config를 불러오고, yml속성을 명시하여 속성에 접근한다.

```ts
import * as config from 'config';


const ymlConfig = config.get('') // ''에 yml 속성을 명시하여 가져옴
```