- Spring Data Rest 프레임워크는 일반적으로 사용하는 [[HTTP]] API가 아닌 깐깐한 Restful API를 빠르게 만들 수 있는 도구이다.

- Restful 원칙을 지키고 간단한 기능의 API를 만드는데는 좋다.
- 하지만, 다양한 View의 요구사항을 만족시키는 HTTP API를 만드는데는 Spring MVC를 사용하는 것이 빠르다.


## build.gradle과 [[application.yaml]] 설정

```yaml
data:  
  rest:  
    base-path: /api # 기본 api 패스 설정  
    detection-strategy: annotated # 기본값은 default 어노테이션을 지정한 곳만 리파지터리를 생성
```


```
implementation 'org.springframework.boot:spring-boot-starter-data-rest'  
implementation 'org.springframework.data:spring-data-rest-hal-explorer' // api를 테스트할 수 있음
```


## 예시

- [[클래스(Class)]]는 [[엔티티(Entity)]]와 [[리파지터리(Repository)]]만 만들어주면 API가 자동 생성된다.

```kotlin
@Entity
@Getter
public class Comment {

  @Id
  @GeneratedValue
  private Long id;

  private String content;

  @ManyToMany // 다대다 관계로 설정
  @JoinTable(
    name = "comment_keyword",
    joinColumns = @JoinColumn(name="comment_id"),
    inverseJoinColumns = @JoinColumn(name="keyword_id")
  )
  private Set<Keyword> keywords = new HashSet<>();

}
```

```java
public interface CommentRepo extends CrudRepository<Comment, Long>{

}
```

- 연관관계인 Keyword Entity도 만들어준다.

```java
@Entity
@Getter
public class Keyword {

  @Id
  @GeneratedValue
  private Long id;

  @Column(unique = true)
  private String name;
  
}
```

```java
public interface KeywordRepo extends CrudRepository<Keyword, Long>{
  
}
```

- 위의 과정으로 구현은 끝났다.

### Entity 조회

- 해당 [[엔티티(Entity)]]의 컬럼([[Spring/SQL/필드(Field)|필드(Field)]])들 뿐만 아니라, 관련된 API 정보까지 반환해주는 Restful 원칙을 지킨 것을 볼 수 있다. 
- 이 점은 프론트엔드를 통해 연관된 API가 무엇인지 쉽게 알 수 있는 유용한 기능이다.
- `GET http://localhost:8080/api/comments`을 통해 조회 가능하다.

```json
{
    "_embedded": {
        "comments": [
            {
                "content": "This is the content.",
                "_links": {
                    "self": {
                        "href": "http://192.168.78.154:8080/api/comments/1"
                    },
                    "comment": {
                        "href": "http://192.168.78.154:8080/api/comments/1"
                    },
                    "keywords": {
                        "href": "http://192.168.78.154:8080/api/comments/1/keywords"
                    }
                }
            }
        ]
    },
    "_links": {
        "self": {
            "href": "http://192.168.78.154:8080/api/comments"
        },
        "profile": {
            "href": "http://192.168.78.154:8080/api/profile/comments"
        }
    }
}
```

### Entity 데이터 추가

```dsconfig
curl --location --request POST 'http://localhost:8080:8080/api/comments' \
--header 'Content-Type: application/json' \
--data-raw '{
    "content": "this is content."
}'
```

```json
{
    "content": "this is content.",
    "_links": {
        "self": {
            "href": "http://192.168.78.154:8080/api/comments/1"
        },
        "comment": {
            "href": "http://192.168.78.154:8080/api/comments/1"
        },
        "keywords": {
            "href": "http://192.168.78.154:8080/api/comments/1/keywords"
        }
    }
}
```

### Entity 관계 추가

- [[엔티티(Entity)]] 간 연관관계를 추가하려면, 먼저 해당 Entity를 추가한다음에 해야한다. 
- Content-Type으로 `text/uri-list`를 사용한다.

```dsconfig
curl --location --request PUT 'http://192.168.78.154:8080/api/comments/1/keywords' \
--header 'Content-Type: text/uri-list' \
--data-raw 'http://192.168.78.154:8080/api/keywords/2
http://192.168.78.154:8080/api/keywords/4'
```

- Response Body는 비어있다.


### Entity 연관 관계 조회

- 해당 [[엔티티(Entity)]]의 [[연관 관계(Relationships)]]를 조회하려면 다른 URI로 조회해야 한다. 
- 컬럼과 연관관계를 함꺼번에 조회하고자 하는 것이 바로 안된다는 점이 불편한 점이다.
- 하지만 이것이 Restful 한 방식, Spring Data REST에서 따로 설정을 해야할 것 같다.

```dsconfig
curl --location --request GET 'http://192.168.78.154:8080/api/comments/1/keywords' \
--header 'Content-Type: text/uri-list'
```

```json
{
    "_embedded": {
        "keywords": [
            {
                "name": "keyword2",
                "_links": {
                    "self": {
                        "href": "http://192.168.78.154:8080/api/keywords/4"
                    },
                    "keyword": {
                        "href": "http://192.168.78.154:8080/api/keywords/4"
                    }
                }
            },
            {
                "name": "keyword1",
                "_links": {
                    "self": {
                        "href": "http://192.168.78.154:8080/api/keywords/2"
                    },
                    "keyword": {
                        "href": "http://192.168.78.154:8080/api/keywords/2"
                    }
                }
            }
        ]
    },
    "_links": {
        "self": {
            "href": "http://192.168.78.154:8080/api/comments/1/keywords"
        }
    }
}
```
