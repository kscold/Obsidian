- Nest.js에서는 [[모듈(Module)]] 생성 명령어를 사용하여 모듈을 생성한다.

## 모듈 생성 문법

```js
nest: using nestcli

module: Schematic that i want to create
controller: controller schematic

name: name of the schematic

--no-spec: 테스트를 위한 코드 생성을 하지 않겠다.
```


## 사용 예시

- 아래 명령어는 boards라는 이름의 module 파일을 생성하는 명령어이다.

```js
nest g module boards
```

- 아래 명령어는 boards라는 이름의 module 파일을 생성하는 명령어이다.

```js
nest g controller boards --no-spec
```


## CLI 종류

| [Schematics] 이름 | 요약  | 설명                                                 |
| --------------- | --- | -------------------------------------------------- |
| app             |     | 단일 저장소 내에서 새 애플리케이션을 생성하십시오(표준 구조인 경우 단일 저장소로 변환). |
| library         | lib | 단일 저장소 내에서 새 라이브러리를 생성합니다(표준 구조인 경우 단일 저장소로 변환).   |
| class           | cl  | 새 클래스를 생성합니다.                                      |
| controller      | co  | 컨트롤러 선언을 생성합니다.                                    |
| decorator       | d   | 사용자 지정 데코레이터를 생성합니다.                               |
| filter          | f   | 필터 선언을 생성합니다.                                      |
| gateway         | ga  | 게이트웨이 선언을 생성합니다.                                   |
| guard           | gu  | 가드 선언을 생성합니다.                                      |
| interface       | itf | 인터페이스를 생성합니다.                                      |
| interceptor     | itc | 인터셉터 선언을 생성합니다.                                    |
| middleware      | mi  | 미들웨어 선언을 생성합니다.                                    |
| module          | mo  | 모듈 선언을 생성합니다.                                      |
| pipe            | pi  | 파이프 선언을 생성합니다.                                     |
| provider        | pr  | 공급자 선언을 생성합니다.                                     |
| resolver        | r   | 확인자 선언을 생성합니다.                                     |
| resource        | res | 새 CRUD 리소스를 생성합니다.                                 |
| service         | s   | 서비스 선언을 생성합니다.                                     |

## CLI로 명령어 입력 시 Controller 등을 만드는 순서

1. CLI는 먼저 name 폴더 찾는다.
2. name 폴더 안에 controller 파일을 생성한다.
3. name 폴더 안에 module 파일을 찾는다.
4. module 파일 안에다가 controller 파일을 넣어준다.


