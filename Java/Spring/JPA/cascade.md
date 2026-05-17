- cascade(영속성 전파)는 [[JPA(Java Persistence API)]]에서 **부모 엔티티에 수행한 작업을 연관된 자식 엔티티에도 자동으로 전파**하는 옵션이다.
- `@OneToMany`, `@ManyToOne` 등의 연관관계 어노테이션에 `cascade` 속성으로 설정한다.

## CascadeType 종류

| CascadeType | 전파 시점 |
| ---- | ---- |
| `PERSIST` | 부모 저장 시 자식도 저장 |
| `MERGE` | 부모 병합(merge) 시 자식도 병합 |
| `REMOVE` | 부모 삭제 시 자식도 삭제 |
| `REFRESH` | 부모 새로고침 시 자식도 새로고침 |
| `DETACH` | 부모 준영속 전환 시 자식도 준영속 |
| `ALL` | 위 5가지 모두 적용 |

## 사용 예시

```java
@Entity
public class User {

    @OneToMany(mappedBy = "user", cascade = CascadeType.ALL)
    private List<Post> posts = new ArrayList<>();
}
```

- `cascade = CascadeType.PERSIST`인 경우, 유저 객체만 `save()`해도 연결된 Post 객체도 함께 저장된다.
- `cascade = CascadeType.REMOVE`인 경우, 유저를 삭제하면 연결된 Post 들도 자동으로 삭제된다.

## cascade REMOVE vs [[orphanRemoval]] 차이

```mermaid
flowchart LR
    A[user.delete()] -->|cascade REMOVE| B[연관된 posts도 삭제]
    C["user.getPosts().remove(post)"] -->|orphanRemoval=true| D[연관 끊긴 post 삭제]
```

| 항목 | `cascade = REMOVE` | `orphanRemoval = true` |
| ---- | ---- | ---- |
| 트리거 | 부모 엔티티 삭제 | 자식이 컬렉션에서 제거될 때 |
| 범위 | `@OneToMany`, `@OneToOne` | `@OneToMany`, `@OneToOne`만 |

## 주의사항

- `CascadeType.ALL`은 편리하지만 삭제 전파까지 되므로 신중하게 사용한다.
- [[orphanRemoval]]이 제대로 동작하려면 `cascade = PERSIST` 이상이 적용되어야 한다.
- 여러 부모가 공유하는 자식 엔티티에는 cascade 적용을 피해야 한다.

## 관련

- [[orphanRemoval]]
- [[@OneToMany]]
- [[@ManyToOne]]
- [[JPA(Java Persistence API)]]
- [[영속성(Persistence)]]
