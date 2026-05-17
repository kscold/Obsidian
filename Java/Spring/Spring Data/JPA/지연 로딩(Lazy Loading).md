- 지연 로딩은 자신과 연관된 [[엔티티(Entity)]]를 실제로 사용할 때 연관된 [[엔티티(Entity)]]를 조회([[SELECT]])하는 방식이다. 
- [[엔티티(Entity)]] A를 조회시 관련(Reference)되어 있는 [[엔티티(Entity)]] B를 한번에 가져오지 않는다. 
- 프록시를 맵핑하고 실제 B를 조회할때 쿼리가 나간다.

- 쿼리가 두 번 나간다. 
- A 조회시 한 번, B 조회시 한 번이다.


## 예시

- 지연 로딩이란, 필요한 시점에 연관된 객체의 테이터를 불러오는 것이다.
- 밑의 코드에서 FetchType을 Lazy로 바꾸면 된다.

```java
// order.java
package jpabook.jpashop.domain;

import lombok.Getter;
import lombok.Setter;

import javax.persistence.*;
import java.time.LocalDateTime;
import java.util.ArrayList;
import java.util.List;


@Entity
@Table(name = "orders")
@Getter @Setter
public class Order {
	
    @Id @GeneratedValue
    @Column(name = "order_id")
    private Long id;
	
    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "member_id")
    private Member member;
	
    @OneToMany(mappedBy = "order", cascade = CascadeType.ALL)
    private List<OrderItem> orderItems = new ArrayList<>();
	
    @OneToOne(fetch = FetchType.LAZY, cascade = CascadeType.ALL)
    @JoinColumn(name = "delivery_id")
    private Delivery delivery;
	
    private LocalDateTime orderDate;
	
    @Enumerated(EnumType.STRING)
    private OrderStatus status; // 주문상태 [order, cancel]
	
    // 연관관계 메서드
    public void setMember(Member member) {
        this.member = member;
        member.getOrders().add(this);
    }
	
    public void addOrderItem(OrderItem orderItem) {
        orderItems.add(orderItem);
        orderItem.setOrder(this);
    }
	
    public void setDelivery(Delivery delivery) {
        this.delivery = delivery;
        delivery.setOrder(this);
    }
}
```

```java
// example.java
package jpabook.jpashop;

import jpabook.jpashop.domain.Order;
import jpabook.jpashop.repository.OrderRepository;
import org.junit.Test;
import org.junit.jupiter.api.DisplayName;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.test.context.junit4.SpringRunner;
import org.springframework.transaction.annotation.Transactional;


@RunWith(SpringRunner.class)
@SpringBootTest
@Transactional
public class example {
	
    @Autowired OrderRepository orderRepository;
	
    @Test
    @DisplayName("지연 로딩")
    public void exam1() {
	    Order result = orderRepository.findOne(1L);
    }

}
```

- 밑의 로그는 테스트 결과이다.

```null
2022-11-10 15:21:24.046  INFO 45242 --- [           main] o.s.t.c.transaction.TransactionContext   : Began transaction (1) for test context [DefaultTestContext@5b367418 testClass = example, testInstance = jpabook.jpashop.example@782ed745, testMethod = exam1@example, testException = [null], mergedContextConfiguration = [WebMergedContextConfiguration@36060e testClass = example, locations = '{}', classes = '{class jpabook.jpashop.JpashopApplication}', contextInitializerClasses = '[]', activeProfiles = '{}', propertySourceLocations = '{}', propertySourceProperties = '{org.springframework.boot.test.context.SpringBootTestContextBootstrapper=true}', contextCustomizers = set[org.springframework.boot.test.autoconfigure.actuate.metrics.MetricsExportContextCustomizerFactory$DisableMetricExportContextCustomizer@63070bab, org.springframework.boot.test.autoconfigure.properties.PropertyMappingContextCustomizer@0, org.springframework.boot.test.autoconfigure.web.servlet.WebDriverContextCustomizerFactory$Customizer@7c1e2a9e, org.springframework.boot.test.context.filter.ExcludeFilterContextCustomizer@2b4bac49, org.springframework.boot.test.json.DuplicateJsonObjectContextCustomizerFactory$DuplicateJsonObjectContextCustomizer@3e96bacf, org.springframework.boot.test.mock.mockito.MockitoContextCustomizer@0, org.springframework.boot.test.web.client.TestRestTemplateContextCustomizer@236e3f4e, org.springframework.boot.test.context.SpringBootTestArgs@1, org.springframework.boot.test.context.SpringBootTestWebEnvironment@4f83df68], resourceBasePath = 'src/main/webapp', contextLoader = 'org.springframework.boot.test.context.SpringBootContextLoader', parent = [null]], attributes = map['org.springframework.test.context.web.ServletTestExecutionListener.activateListener' -> true, 'org.springframework.test.context.web.ServletTestExecutionListener.populatedRequestContextHolder' -> true, 'org.springframework.test.context.web.ServletTestExecutionListener.resetRequestContextHolder' -> true, 'org.springframework.test.context.event.ApplicationEventsTestExecutionListener.recordApplicationEvents' -> false]]; transaction manager [org.springframework.orm.jpa.JpaTransactionManager@7c6fc278]; rollback [true]
2022-11-10 15:21:24.112 DEBUG 45242 --- [           main] org.hibernate.SQL                        : 
    select
        order0_.order_id as order_id1_6_0_,
        order0_.delivery_id as delivery4_6_0_,
        order0_.member_id as member_i5_6_0_,
        order0_.order_date as order_da2_6_0_,
        order0_.status as status3_6_0_ 
    from
        orders order0_ 
    where
        order0_.order_id=?
Hibernate: 
    select
        order0_.order_id as order_id1_6_0_,
        order0_.delivery_id as delivery4_6_0_,
        order0_.member_id as member_i5_6_0_,
        order0_.order_date as order_da2_6_0_,
        order0_.status as status3_6_0_ 
    from
        orders order0_ 
    where
        order0_.order_id=?
2022-11-10 15:21:24.115 TRACE 45242 --- [           main] o.h.type.descriptor.sql.BasicBinder      : binding parameter [1] as [BIGINT] - [1]
2022-11-10 15:21:24.119  INFO 45242 --- [           main] p6spy                                    : #1668061284119 | took 3ms | statement | connection 3| url jdbc:h2:mem:testdb
select order0_.order_id as order_id1_6_0_, order0_.delivery_id as delivery4_6_0_, order0_.member_id as member_i5_6_0_, order0_.order_date as order_da2_6_0_, order0_.status as status3_6_0_ from orders order0_ where order0_.order_id=?
select order0_.order_id as order_id1_6_0_, order0_.delivery_id as delivery4_6_0_, order0_.member_id as member_i5_6_0_, order0_.order_date as order_da2_6_0_, order0_.status as status3_6_0_ from orders order0_ where order0_.order_id=1;
2022-11-10 15:21:24.125  INFO 45242 --- [           main] p6spy                                    : #1668061284125 | took 0ms | rollback | connection 3| url jdbc:h2:mem:testdb

;
2022-11-10 15:21:24.127  INFO 45242 --- [           main] o.s.t.c.transaction.TransactionContext   : Rolled back transaction for test: [DefaultTestContext@5b367418 testClass = example, testInstance = jpabook.jpashop.example@782ed745, testMethod = exam1@example, testException = [null], mergedContextConfiguration = [WebMergedContextConfiguration@36060e testClass = example, locations = '{}', classes = '{class jpabook.jpashop.JpashopApplication}', contextInitializerClasses = '[]', activeProfiles = '{}', propertySourceLocations = '{}', propertySourceProperties = '{org.springframework.boot.test.context.SpringBootTestContextBootstrapper=true}', contextCustomizers = set[org.springframework.boot.test.autoconfigure.actuate.metrics.MetricsExportContextCustomizerFactory$DisableMetricExportContextCustomizer@63070bab, org.springframework.boot.test.autoconfigure.properties.PropertyMappingContextCustomizer@0, org.springframework.boot.test.autoconfigure.web.servlet.WebDriverContextCustomizerFactory$Customizer@7c1e2a9e, org.springframework.boot.test.context.filter.ExcludeFilterContextCustomizer@2b4bac49, org.springframework.boot.test.json.DuplicateJsonObjectContextCustomizerFactory$DuplicateJsonObjectContextCustomizer@3e96bacf, org.springframework.boot.test.mock.mockito.MockitoContextCustomizer@0, org.springframework.boot.test.web.client.TestRestTemplateContextCustomizer@236e3f4e, org.springframework.boot.test.context.SpringBootTestArgs@1, org.springframework.boot.test.context.SpringBootTestWebEnvironment@4f83df68], resourceBasePath = 'src/main/webapp', contextLoader = 'org.springframework.boot.test.context.SpringBootContextLoader', parent = [null]], attributes = map['org.springframework.test.context.web.ServletTestExecutionListener.activateListener' -> true, 'org.springframework.test.context.web.ServletTestExecutionListener.populatedRequestContextHolder' -> true, 'org.springframework.test.context.web.ServletTestExecutionListener.resetRequestContextHolder' -> true, 'org.springframework.test.context.event.ApplicationEventsTestExecutionListener.recordApplicationEvents' -> false]]
```


- order를 조회해도 order만 조회하는 쿼리만 생성이 된다.
- 나머지 연관 객체들을 조회하는 쿼리는 생성되지 않는다.

추가)

### ✔ ️지연 로딩은 어떻게 이루어질 수 있는가? -> "프록시 객체"

객체 조회 시 항상 연관된 객체까지 함께 조회하는 것은 효율적이지 않다.  
그래서 JPA는 엔티티가 실제로 사용되기 전까지 데이터베이스 조회를 지연할 수 있도록 제공하는데 이를 **"지연 로딩"** 이라고 한다.

> "실제로 사용하는 시점에 데이터베이스에서 필요한 데이터를 가져오는 것"

하지만 지연 로딩을 사용하면 실제 엔티티 객체 대신 가짜 객체가 필요한데, 이것이 바로 **"프록시 객체"**

ManyToOne 관계인 Order(N) - Member(1) 를 예로 들어 보자.

엔티티 Order 조회 시 관련 되어 있는 엔티티 Member를 한번에 가져오지 않는다.  
프록시를 맵핑하고 실제 Member를 조회할 때 쿼리가 나간다.

![](https://github.com/GDSC-Hongik/GDSC-1st-Backend-Study/blob/master/ssssujini99/jpashop/markdown/img2.jpeg?raw=true!%5B%5D(https://velog.velcdn.com/images/ssssujini99/post/3398ee9b-d2a3-4279-a7db-481682a88e23/image.png))

- 지연로딩을 설정해놓으면, Order find 메소드 호출 시 Member는 Proxy 상태이다.
    
- order.getMember(); 메소드를 호출하는 순간 JPA는 DB를 조회하여 Proxy에 값을 채운다. (바꾸는 게 아니라 값을 채워넣는 것!)
    
- 프록시는 실제 엔티티를 상속받은 오브젝트이다. 따라서 Member 엔티티와 겉모양이 같고 JPA는 이 프록시(Target)에 값을 채워넣는 것이다.