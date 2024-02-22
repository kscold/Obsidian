- @CreatedDate [[어노테이션(Annotation)]]은 Spring Data [[JPA(Java Persistence API)]]의 [[Auditing]]에서 가장 흥미로운 어노테이션이다.
- 사실 이 어노테이션도 Spring Data JPA 의 고유 기능은 아니고 Spring Data 에 있는 어노테이션으로 Spring Data 에서 추상화 해놓은 것이다.

- 생성일을 기록하기 위해 `LocalDateTime` 타입의 필드에 `@CreateDate` 를 적용한다. 
- 또한 생성일자는 수정되어서는 안되므로 `@Column(updatable = false)` 를 적용한다. 이렇게 적용하면, 엔티티가 생성됨을 감지하고 그 시점을 `createdAt` 필드에 기록한다.

- [[@CreatedBy]]

## 예시

```java
import lombok.Getter;
import org.springframework.data.annotation.CreatedDate;
import org.springframework.data.annotation.LastModifiedDate;
import org.springframework.data.jpa.domain.support.AuditingEntityListener;

import javax.persistence.EntityListeners;
import javax.persistence.MappedSuperclass;
import java.time.LocalDateTime;

@Getter
@MappedSuperclass
@EntityListeners(AuditingEntityListener.class)
public class BaseTime {

    @CreatedDate
    private LocalDateTime createdDate;

    @LastModifiedDate
    private LocalDateTime modifiedDate;
}
```

많은 테이블에서 공통적으로 사용될 컬럼인 생성된 시간과 수정된 시간을 필드로 가진 BaseTime Class입니다.

대부분의 테이블에서 공통적으로 사용될 컬럼이니만큼 각각의 Entity에 생성하는 것보다 부모 클래스로 만들어 상속받아서 사용하는 것이 효율적이라 그렇게 많이 사용되고 있습니다.