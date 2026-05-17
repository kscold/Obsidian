- `JSONB`는 PostgreSQL에서 JSON 데이터를 **바이너리(Binary) 형태로 저장**하는 타입이다.
- 일반 `JSON` 타입(텍스트 저장)과 달리 파싱 없이 직접 조회·인덱싱이 가능하다.
- 키-값 쌍의 순서가 보장되지 않고 중복 키는 마지막 값만 유지된다.

## JSON vs JSONB

| 항목 | `JSON` | `JSONB` |
| ---- | ---- | ---- |
| 저장 방식 | 텍스트 그대로 | 바이너리 파싱 후 저장 |
| 삽입 속도 | 빠름 | 약간 느림 (파싱 오버헤드) |
| 조회 속도 | 느림 | **빠름** |
| 인덱스 | 불가 | GIN/BTREE 인덱스 가능 |
| 키 순서 보존 | 보존 | 보존 안 함 |
| 중복 키 | 허용 | 마지막 값만 유지 |

- 쓰기보다 읽기가 많은 일반 서비스에서는 `JSONB`가 적합하다.

## 기본 사용

```sql
CREATE TABLE products (
    id SERIAL PRIMARY KEY,
    name VARCHAR(200),
    metadata JSONB
);

INSERT INTO products (name, metadata) VALUES
  ('상품A', '{"color": "red", "tags": ["sale", "new"]}');

-- JSON 필드 조회
SELECT name, metadata->>'color' AS color FROM products;
-- metadata->>'color'  : 텍스트로 반환
-- metadata->'color'   : JSON 값으로 반환

-- 배열 요소 접근
SELECT metadata->'tags'->0 FROM products;
-- "sale"
```

## 인덱스 (GIN)

```sql
-- GIN 인덱스로 JSONB 전체 검색 최적화
CREATE INDEX idx_products_metadata ON products USING GIN (metadata);

-- 특정 키에 포함 여부 검색
SELECT * FROM products WHERE metadata @> '{"tags": ["sale"]}';
```

## JPA에서 JSONB 사용

- JPA는 기본적으로 JSONB를 지원하지 않으므로 Hibernate 커스텀 타입 또는 라이브러리가 필요하다.

```gradle
// hypersistence-utils 라이브러리 사용 (권장)
implementation 'io.hypersistence:hypersistence-utils-hibernate-63:3.7.0'
```

```java
@Entity
@Table(name = "products")
public class Product {

    @Id @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Type(JsonBinaryType.class)
    @Column(columnDefinition = "jsonb")
    private Map<String, Object> metadata;
}
```

## 주요 연산자

| 연산자 | 설명 | 예시 |
| ---- | ---- | ---- |
| `->` | 키로 JSON 값 반환 | `metadata->'color'` |
| `->>` | 키로 텍스트 반환 | `metadata->>'color'` |
| `@>` | 오른쪽 JSON을 포함하는지 | `metadata @> '{"sale": true}'` |
| `<@` | 왼쪽이 오른쪽 JSON에 포함되는지 | |
| `?` | 키가 존재하는지 | `metadata ? 'color'` |
| `#>` | 경로로 값 접근 | `metadata #> '{address, city}'` |

## 관련

- [[PostgreSQL]]
- [[관계형 데이터베이스(Relational DataBase)]]
