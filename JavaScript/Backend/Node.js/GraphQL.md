- GraphQL은 Facebook이 개발한 API 쿼리 언어이자 런타임으로, 클라이언트가 필요한 데이터를 정확하게 요청할 수 있다.
- [[REST API]]는 엔드포인트별로 응답 형태가 고정되어 있지만, GraphQL은 **단일 엔드포인트**(`/graphql`)에서 클라이언트가 원하는 필드만 요청한다.
- 과조회(Over-fetching)와 미조회(Under-fetching) 문제를 해결한다.

## REST vs GraphQL 비교

| 구분 | REST API | GraphQL |
|------|---------|---------|
| 엔드포인트 | 리소스마다 별도 | 단일 `/graphql` |
| 데이터 선택 | 서버 결정 (고정) | 클라이언트 선택 (유연) |
| Over-fetching | 발생 가능 | 없음 |
| Under-fetching | N+1 문제 발생 | 한 번에 해결 |
| 타입 시스템 | 없음 (Swagger 별도) | 스키마 기반 강타입 |

## 기본 개념

- **Schema**: GraphQL 타입 정의. 데이터 구조와 쿼리/뮤테이션을 선언한다.
- **Query**: 데이터 조회 요청 (READ)
- **Mutation**: 데이터 변경 요청 (CREATE/UPDATE/DELETE)
- **Subscription**: 실시간 데이터 구독 ([[비동기(asynchronous)]] WebSocket 기반)
- **Resolver**: 각 필드의 실제 데이터를 반환하는 함수

## 스키마 정의와 쿼리 예시

```graphql
# 타입 정의 (Schema)
type User {
  id: ID!
  name: String!
  email: String!
  posts: [Post!]!
}

type Post {
  id: ID!
  title: String!
  author: User!
}

type Query {
  user(id: ID!): User
  users: [User!]!
}

type Mutation {
  createUser(name: String!, email: String!): User!
}
```

```graphql
# 클라이언트 쿼리 — 필요한 필드만 요청
query {
  user(id: "1") {
    name
    email
    posts {
      title
    }
  }
}

# 응답
{
  "data": {
    "user": {
      "name": "홍길동",
      "email": "hong@example.com",
      "posts": [
        { "title": "첫 번째 글" }
      ]
    }
  }
}
```

- 응답은 [[JSON]] 형식으로 반환되며, 요청한 필드만 정확히 포함된다.
- `!`는 non-null(필수값)을 의미하고, `[Post!]!`는 null이 없는 배열 자체도 null이 아님을 의미한다.

## NestJS에서 GraphQL 사용

- [[Backend/NestJS/NestJS|NestJS]]에서는 `@nestjs/graphql` 패키지로 GraphQL을 통합한다.
- `@Resolver()`, `@Query()`, `@Mutation()` 데코레이터로 리졸버를 선언한다.
- Code-first 방식(TypeScript 클래스로 스키마 자동 생성)과 Schema-first 방식(`.graphql` 파일 직접 작성) 두 가지를 지원한다.

```ts
import { Resolver, Query, Args } from '@nestjs/graphql';

@Resolver(() => User)
export class UserResolver {
  constructor(private readonly userService: UserService) {}

  @Query(() => User)
  async user(@Args('id') id: string): Promise<User> {
    return this.userService.findById(id);
  }
}
```
