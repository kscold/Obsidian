- 즉시 로딩 [[엔티티(Entity)]]를 조회할 때 자신과 연관되는 [[엔티티(Entity)]]를 조인([[JOIN]])을 통해 함께 조회하는 방식한다.

- 엔티티 A 조회시 관련되어 있는 엔티티 B를 같이 가져온다. 
- 실제 엔티티를 맵핑한다. 
- [[JOIN]]을 사용하여 한번에 가져온다.
- A [[JOIN]] B, 쿼리가 한 번만 나간다.


## 예시

- 즉시 로딩이란, 데이터를 조회할 때 연관된 모든 객체의 데이터까지 한 번에 불러오는 것이다.

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
@Getter
@Setter
public class Order {
	
    @Id
    @GeneratedValue
    @Column(name = "order_id")
    private Long id;
	
    @ManyToOne
    @JoinColumn(name = "member_id")
    private Member member;
	
    @OneToMany(mappedBy = "order", cascade = CascadeType.ALL)
    private List<OrderItem> orderItems = new ArrayList<>();
	
    @OneToOne(cascade = CascadeType.ALL)
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

- 해당 예시에서 보면 Order - Member 는 [[다대일(ManyToOne)]] 매핑 관계이고 Order - Delivery는 일대일(OneToOne) 매핑 관계이다.

![](https://github.com/GDSC-Hongik/GDSC-1st-Backend-Study/blob/master/ssssujini99/jpashop/markdown/img.png?raw=true)

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
    @DisplayName("즉시 로딩은 연관관계를 맺고있는 엔티티는 모두 조회가 된다")
    public void exam1() {
		Order result = orderRepository.findOne(1L);
    }
}
```

- 밑에는 테스트 실행 결과이다.

```null
2022-11-10 15:11:19.498  INFO 43197 --- [           main] o.s.t.c.transaction.TransactionContext   : Began transaction (1) for test context [DefaultTestContext@5b367418 testClass = example, testInstance = jpabook.jpashop.example@50aa9a91, testMethod = exam1@example, testException = [null], mergedContextConfiguration = [WebMergedContextConfiguration@36060e testClass = example, locations = '{}', classes = '{class jpabook.jpashop.JpashopApplication}', contextInitializerClasses = '[]', activeProfiles = '{}', propertySourceLocations = '{}', propertySourceProperties = '{org.springframework.boot.test.context.SpringBootTestContextBootstrapper=true}', contextCustomizers = set[org.springframework.boot.test.autoconfigure.actuate.metrics.MetricsExportContextCustomizerFactory$DisableMetricExportContextCustomizer@63070bab, org.springframework.boot.test.autoconfigure.properties.PropertyMappingContextCustomizer@0, org.springframework.boot.test.autoconfigure.web.servlet.WebDriverContextCustomizerFactory$Customizer@7c1e2a9e, org.springframework.boot.test.context.filter.ExcludeFilterContextCustomizer@2b4bac49, org.springframework.boot.test.json.DuplicateJsonObjectContextCustomizerFactory$DuplicateJsonObjectContextCustomizer@3e96bacf, org.springframework.boot.test.mock.mockito.MockitoContextCustomizer@0, org.springframework.boot.test.web.client.TestRestTemplateContextCustomizer@236e3f4e, org.springframework.boot.test.context.SpringBootTestArgs@1, org.springframework.boot.test.context.SpringBootTestWebEnvironment@4f83df68], resourceBasePath = 'src/main/webapp', contextLoader = 'org.springframework.boot.test.context.SpringBootContextLoader', parent = [null]], attributes = map['org.springframework.test.context.web.ServletTestExecutionListener.activateListener' -> true, 'org.springframework.test.context.web.ServletTestExecutionListener.populatedRequestContextHolder' -> true, 'org.springframework.test.context.web.ServletTestExecutionListener.resetRequestContextHolder' -> true, 'org.springframework.test.context.event.ApplicationEventsTestExecutionListener.recordApplicationEvents' -> false]]; transaction manager [org.springframework.orm.jpa.JpaTransactionManager@63af52a6]; rollback [true]
2022-11-10 15:11:19.556 DEBUG 43197 --- [           main] org.hibernate.SQL                        :
select
order0_.order_id as order_id1_6_0_,
order0_.delivery_id as delivery4_6_0_,
order0_.member_id as member_i5_6_0_,
order0_.order_date as order_da2_6_0_,
order0_.status as status3_6_0_,
delivery1_.delivery_id as delivery1_2_1_,
delivery1_.city as city2_2_1_,
delivery1_.street as street3_2_1_,
delivery1_.zipcode as zipcode4_2_1_,
delivery1_.status as status5_2_1_,
member2_.member_id as member_i1_4_2_,
member2_.city as city2_4_2_,
member2_.street as street3_4_2_,
member2_.zipcode as zipcode4_4_2_,
member2_.name as name5_4_2_
from
orders order0_
left outer join
delivery delivery1_
on order0_.delivery_id=delivery1_.delivery_id
left outer join
member member2_
on order0_.member_id=member2_.member_id
where
order0_.order_id=?
Hibernate:
select
order0_.order_id as order_id1_6_0_,
order0_.delivery_id as delivery4_6_0_,
order0_.member_id as member_i5_6_0_,
order0_.order_date as order_da2_6_0_,
order0_.status as status3_6_0_,
delivery1_.delivery_id as delivery1_2_1_,
delivery1_.city as city2_2_1_,
delivery1_.street as street3_2_1_,
delivery1_.zipcode as zipcode4_2_1_,
delivery1_.status as status5_2_1_,
member2_.member_id as member_i1_4_2_,
member2_.city as city2_4_2_,
member2_.street as street3_4_2_,
member2_.zipcode as zipcode4_4_2_,
member2_.name as name5_4_2_
from
orders order0_
left outer join
delivery delivery1_
on order0_.delivery_id=delivery1_.delivery_id
left outer join
member member2_
on order0_.member_id=member2_.member_id
where
order0_.order_id=?
2022-11-10 15:11:19.560 TRACE 43197 --- [           main] o.h.type.descriptor.sql.BasicBinder      : binding parameter [1] as [BIGINT] - [1]
2022-11-10 15:11:19.564  INFO 43197 --- [           main] p6spy                                    : #1668060679564 | took 3ms | statement | connection 3| url jdbc:h2:mem:testdb
select order0_.order_id as order_id1_6_0_, order0_.delivery_id as delivery4_6_0_, order0_.member_id as member_i5_6_0_, order0_.order_date as order_da2_6_0_, order0_.status as status3_6_0_, delivery1_.delivery_id as delivery1_2_1_, delivery1_.city as city2_2_1_, delivery1_.street as street3_2_1_, delivery1_.zipcode as zipcode4_2_1_, delivery1_.status as status5_2_1_, member2_.member_id as member_i1_4_2_, member2_.city as city2_4_2_, member2_.street as street3_4_2_, member2_.zipcode as zipcode4_4_2_, member2_.name as name5_4_2_ from orders order0_ left outer join delivery delivery1_ on order0_.delivery_id=delivery1_.delivery_id left outer join member member2_ on order0_.member_id=member2_.member_id where order0_.order_id=?
select order0_.order_id as order_id1_6_0_, order0_.delivery_id as delivery4_6_0_, order0_.member_id as member_i5_6_0_, order0_.order_date as order_da2_6_0_, order0_.status as status3_6_0_, delivery1_.delivery_id as delivery1_2_1_, delivery1_.city as city2_2_1_, delivery1_.street as street3_2_1_, delivery1_.zipcode as zipcode4_2_1_, delivery1_.status as status5_2_1_, member2_.member_id as member_i1_4_2_, member2_.city as city2_4_2_, member2_.street as street3_4_2_, member2_.zipcode as zipcode4_4_2_, member2_.name as name5_4_2_ from orders order0_ left outer join delivery delivery1_ on order0_.delivery_id=delivery1_.delivery_id left outer join member member2_ on order0_.member_id=member2_.member_id where order0_.order_id=1;
2022-11-10 15:11:19.569  INFO 43197 --- [           main] p6spy                                    : #1668060679569 | took 0ms | rollback | connection 3| url jdbc:h2:mem:testdb

;
2022-11-10 15:11:19.570  INFO 43197 --- [           main] o.s.t.c.transaction.TransactionContext   : Rolled back transaction for test: [DefaultTestContext@5b367418 testClass = example, testInstance = jpabook.jpashop.example@50aa9a91, testMethod = exam1@example, testException = [null], mergedContextConfiguration = [WebMergedContextConfiguration@36060e testClass = example, locations = '{}', classes = '{class jpabook.jpashop.JpashopApplication}', contextInitializerClasses = '[]', activeProfiles = '{}', propertySourceLocations = '{}', propertySourceProperties = '{org.springframework.boot.test.context.SpringBootTestContextBootstrapper=true}', contextCustomizers = set[org.springframework.boot.test.autoconfigure.actuate.metrics.MetricsExportContextCustomizerFactory$DisableMetricExportContextCustomizer@63070bab, org.springframework.boot.test.autoconfigure.properties.PropertyMappingContextCustomizer@0, org.springframework.boot.test.autoconfigure.web.servlet.WebDriverContextCustomizerFactory$Customizer@7c1e2a9e, org.springframework.boot.test.context.filter.ExcludeFilterContextCustomizer@2b4bac49, org.springframework.boot.test.json.DuplicateJsonObjectContextCustomizerFactory$DuplicateJsonObjectContextCustomizer@3e96bacf, org.springframework.boot.test.mock.mockito.MockitoContextCustomizer@0, org.springframework.boot.test.web.client.TestRestTemplateContextCustomizer@236e3f4e, org.springframework.boot.test.context.SpringBootTestArgs@1, org.springframework.boot.test.context.SpringBootTestWebEnvironment@4f83df68], resourceBasePath = 'src/main/webapp', contextLoader = 'org.springframework.boot.test.context.SpringBootContextLoader', parent = [null]], attributes = map['org.springframework.test.context.web.ServletTestExecutionListener.activateListener' -> true, 'org.springframework.test.context.web.ServletTestExecutionListener.populatedRequestContextHolder' -> true, 'org.springframework.test.context.web.ServletTestExecutionListener.resetRequestContextHolder' -> true, 'org.springframework.test.context.event.ApplicationEventsTestExecutionListener.recordApplicationEvents' -> false]]
```


- 위의 실행결과는 주문과 N:1 매핑관계인 회원 관계까지 모두 조회된다.
- 주문과 1:1 매핑 관계인 배송 관계까지 모두 조회된다.

- 즉시 로딩(Eager Loading)을 방식을 사용하면 Order를 조회하는 시점에 바로 Member와 Delivery까지 불러오는 [[쿼리(query)]]를 날려 한꺼번에 데이터를 불러오는 것을 확인할 수 있다.