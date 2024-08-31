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


## CLI로 명령어 입력 시 Controller 등을 만드는 순서

1. CLI는 먼저 name 폴더 찾는다.
2. name 폴더 안에 controller 파일을 생성한다.
3. name 폴더 안에 module 파일을 찾는다.
4. module 파일 안에다가 controller 파일을 넣어준다.


