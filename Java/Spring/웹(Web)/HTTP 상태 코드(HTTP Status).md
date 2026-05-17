- HTTP 상태 코드는 클라이언트가 보낸 요청에 대해 **서버가 처리 결과를 3자리 숫자로 응답**하는 코드이다.
- 100번대~500번대, 5개 그룹으로 구분된다.
- Spring에서는 `HttpStatus` enum 클래스로 정의되어 있으며, `ResponseEntity.status(HttpStatus.OK)` 형태로 사용한다.

## 2XX — 성공

| 코드 | 이름 | 설명 | 주로 사용되는 상황 |
| ---- | ---- | ---- | ---- |
| 200 | OK | 요청 성공 | GET 조회 성공 |
| 201 | Created | 리소스 생성 성공 | POST로 생성 후 응답 |
| 204 | No Content | 성공했지만 응답 바디 없음 | DELETE, PUT 성공 |

## 3XX — 리다이렉션

| 코드 | 이름 | 설명 |
| ---- | ---- | ---- |
| 301 | Moved Permanently | 영구 이동 (URL 변경) |
| 302 | Found | 임시 이동 |
| 304 | Not Modified | 캐시된 응답 사용 |

## 4XX — 클라이언트 오류

| 코드 | 이름 | 설명 | 주로 사용되는 상황 |
| ---- | ---- | ---- | ---- |
| 400 | Bad Request | 잘못된 요청 | 파라미터 누락, 유효성 실패 |
| 401 | Unauthorized | 인증 실패 | 로그인 안 됨, 토큰 없음/만료 |
| 403 | Forbidden | 인가 실패 | 권한 없음 (로그인은 됐지만 접근 불가) |
| 404 | Not Found | 리소스 없음 | 존재하지 않는 URL 또는 데이터 |
| 405 | Method Not Allowed | HTTP 메서드 불일치 | GET 엔드포인트에 POST 요청 |
| 409 | Conflict | 리소스 충돌 | 중복 데이터(slug, email 등) |
| 422 | Unprocessable Entity | 요청 형식은 맞지만 처리 불가 | 비즈니스 규칙 위반 |
| 429 | Too Many Requests | 요청 횟수 초과 | Rate Limiting |

## 5XX — 서버 오류

| 코드 | 이름 | 설명 | 주로 사용되는 상황 |
| ---- | ---- | ---- | ---- |
| 500 | Internal Server Error | 서버 내부 오류 | 예외 처리 안 된 에러 |
| 502 | Bad Gateway | 게이트웨이/프록시 오류 | 외부 API 연결 실패 |
| 503 | Service Unavailable | 서비스 불가 | 서버 다운, 유지보수 중 |

## Spring에서 사용

```java
// ResponseEntity 직접 사용
return ResponseEntity.ok(data);                          // 200
return ResponseEntity.created(uri).body(data);           // 201
return ResponseEntity.noContent().build();               // 204
return ResponseEntity.badRequest().body(error);          // 400
return ResponseEntity.status(HttpStatus.FORBIDDEN).body(error);  // 403

// GlobalExceptionHandler 연동
@ExceptionHandler(BusinessException.class)
public ResponseEntity<ApiResponse<Void>> handle(BusinessException e) {
    return ResponseEntity
        .status(e.getErrorCode().getStatus())   // ErrorCode에 정의된 HttpStatus 사용
        .body(ApiResponse.error(e));
}
```

## 401 vs 403 구분

| 상황 | 코드 | 설명 |
| ---- | ---- | ---- |
| 토큰이 없거나 만료됨 | 401 | 인증(Authentication) 실패 |
| 토큰은 있지만 권한 부족 | 403 | 인가(Authorization) 실패 |

## 관련

- [[HTTP]]
- [[HttpStatus]]
- [[GlobalExceptionHandler]]
- [[BusinessException]]
- [[인증(Authentication)]]
- [[인가(Authorization)]]
- [[ResponseEntity]]
