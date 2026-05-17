- MySQL은 세계에서 가장 널리 쓰이는 오픈소스 관계형 데이터베이스이다.
- MariaDB는 MySQL 창시자(Monty Widenius)가 오라클의 MySQL 인수에 반발해 만든 **MySQL 포크**로, 대부분의 API와 명령어가 MySQL과 호환된다.
- Spring Boot에서는 주로 `spring-boot-starter-data-jpa` + MySQL/MariaDB 드라이버를 함께 사용한다.

## MySQL vs MariaDB 주요 차이

| 항목 | MySQL | MariaDB |
| ---- | ---- | ---- |
| 관리 주체 | Oracle | MariaDB Foundation |
| 라이선스 | GPL + Commercial | GPL |
| 스토리지 엔진 | InnoDB (기본) | InnoDB + Aria 등 추가 엔진 |
| JSON 지원 | 5.7+ (네이티브) | 10.2+ (별도 구현) |
| 성능 | 유사 | 일부 쿼리 더 빠른 경우 있음 |
| 호환성 | - | MySQL 호환 |

## Spring Boot 연동

```yaml
# application.yml (MySQL)
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/mydb?serverTimezone=Asia/Seoul&characterEncoding=UTF-8
    username: root
    password: password
    driver-class-name: com.mysql.cj.jdbc.Driver
  jpa:
    database-platform: org.hibernate.dialect.MySQLDialect
    hibernate:
      ddl-auto: validate   # 운영: validate / 개발: update
    show-sql: false
```

```yaml
# application.yml (MariaDB)
spring:
  datasource:
    url: jdbc:mariadb://localhost:3306/mydb?serverTimezone=Asia/Seoul
    username: root
    password: password
    driver-class-name: org.mariadb.jdbc.Driver
  jpa:
    database-platform: org.hibernate.dialect.MariaDBDialect
```

```gradle
// build.gradle 의존성
// MySQL
runtimeOnly 'com.mysql:mysql-connector-j'

// MariaDB
runtimeOnly 'org.mariadb.jdbc:mariadb-java-client'
```

## DDL 자동 설정 (ddl-auto)

| 값 | 동작 | 권장 환경 |
| ---- | ---- | ---- |
| `create` | 시작 시 테이블 삭제 후 재생성 | 로컬 개발 초기 |
| `update` | 스키마 변경 사항 반영 (데이터 유지) | 로컬 개발 |
| `validate` | 스키마 검증만, 변경하지 않음 | 스테이징/운영 |
| `none` | 아무 작업 안 함 | 운영 (마이그레이션 도구 사용 시) |

## 관련

- [[관계형 데이터베이스(Relational DataBase)]]
- [[데이터 타입(Data Type)]]
- [[인덱스(Index)]]
- [[트랜잭션(Transaction)]]
- [[정규화(Normalization)]]
- [[JPA(Java Persistence API)]]
