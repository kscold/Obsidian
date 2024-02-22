- 데이터 정의어(Data Definition Language, DDL)은 데이터베이스 [[테이블(Table)]]([[스키마(Schema)]])의 생성, 변경, 삭제를 담당하는 명령어이다.
- 대표적으로 [[CREATE]], ALTER, [[DROP]], RENAME, TRUNCATE 가 있다.

- DDL 말고도 DML, DCL 이 있다.

## DDL 자동 생성 

- ddl-auto는 데이터베이스의 [[테이블(Table)]]을 [[JPA(Java Persistence API)]] 의 구현체가 자동으로 생성해주는 기능을 말한다.
- 테이블 자동 생성 전략이라고 불린다.

- 데이터베이스에는 다양한 dialect가 존재하는데 JPA 구현체는 application.[[yaml]]에 지정되어 있는 dialect에 맞는 적정한 DDL을 생성한다.

### ddl-auto 속성

|속성|기능|
|---|---|
|create|기존 테이블을 삭제 후 다시 생성한다.|
|create-drop|create + 서버 종료시 테이블 삭제한다.|
|update|변경 부분만 반영한다.|
|validate|엔티티와 테이블이 정상 매핑 되었는지만 확인한다.|
|none|아무 기능도 하지 않는다.|

- ddl-auto는 이렇게 5가지 속성을 제공하며 해당 기능을 통해 개발자는 직접 DDL 을 생성할 필요 없이 데이터베이스를 관리할 수 있게 된다.

## ddl-auto 사용법

- ddl-auto 은 application-properties(yaml)을 통해 사용할 수 있다.

```yaml
spring:
  h2:
    console:
      enabled: true
  datasource:
    driver-class-name: org.h2.Driver
    url: jdbc:h2:mem:test
    username: sa
    password: 1111

  jpa:
    open-in-view: false
    hibernate:
      ddl-auto: create // spring.jpa.ddl-auto 를 통해 설정 가능
    properties:
      hibernate:
        show_sql: true
        format_sql: true
    defer-datasource-initialization: true
```

## 올바른 ddl-auto 사용법

- 위에서 알아본바와 같이 ddl-auto는 개발자의 수고를 덜어줄 수 있는 기능이다.
- 하지만 각별한 주의를 기울여 사용해야한다.
- 왜냐하면 해당 기능을 잘못된 경우에 사용시 중요한 데이터가 날아갈 수 있으며, 서버에 큰 에러가 발생할 수도 있기 때문이다.

- 실제 운영 서버에서는 `create`, `create-drop`, `update` 절때 사용하지 않는다.
- 개발 초기 단계에서는 `create`, `update` 사용한다.
- 테스트 서버에서는 `update`,`validate` 사용한다.
- 스테이징 서버와 운영 서버에서는 `validate` , `none` 사용한다.

- 실제 운영 서버에서는 직접 [[쿼리(query)]]를 날리는 것이 좋다.