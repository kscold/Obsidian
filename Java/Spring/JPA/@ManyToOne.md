- @ManyToOne [[어노테이션(Annotation)]]은 @OneToMany와 크게 다르지 않다.
- 다만 @OneToMany가 1:N이라고 한다면 @ManyToOne은 N:1 관계라고 보면 된다


## @ManyToOne 속성

- [[관계형 데이터베이스(Relational DataBase)]]에서 사용한다.
- @ManyToOne 속성에는 다음과 같은 것들이 있다.
	- targetEntity
	- cascade
	- fetch
	- optional


### optional

- 이 항목은 false로 설정했을 때 해당 [[객체(Object)]]에 null이 들어갈 수 있다.
- 반대로 반드시 값이 필요하다면 true가 들어간다.
- 기본값은 true이다.

## 예시

- 현실세계에서 예를 들면 회원과 핸드폰의 관계에서 핸드폰을 보면 된다.
- 핸드폰은 자신을 소유한 회원이 있다. 
- 하지만 이 회원은 핸드폰을 여러개 소지할 수도 있고 하나만 소지할 수도 있다.
- 회원쪽에서 핸드폰을 바라본다면 @OneToMany 관계지만 핸드폰이 회원을 바라본다면 @ManyToOne이 된다.

- 테이블은 아래처럼 구성 되어있다.
- MariaDB을 사용했다.

```sql
CREATE TABLE `member` (
	`seq` INT(10) NOT NULL AUTO_INCREMENT,
	`name` VARCHAR(50) NULL DEFAULT NULL,
	PRIMARY KEY (`seq`)
)

COLLATE='utf8_general_ci'
ENGINE=InnoDB
AUTO_INCREMENT=1;

CREATE TABLE `phone` (
	`seq` INT(10) NOT NULL AUTO_INCREMENT,
	`member_id` INT(10) NULL DEFAULT NULL,
	`no` VARCHAR(50) NULL DEFAULT NULL,
	PRIMARY KEY (`seq`)
)

COLLATE='utf8_general_ci'
ENGINE=InnoDB
AUTO_INCREMENT=1;
```

- 회원 Entity 및 핸드폰 Entity를 만든다.

```java
// Member Entity
package jpa3;

import javax.persistence.Column;
import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;

@Entity
public class Member {
	
	@Id
	@Column(name="seq") // seq 필드를 선택
	@GeneratedValue(strategy=GenerationType.AUTO) // DB가 기본키로 seq 자동 선택
	private int seq;
	
	@Column(name="name")
	private String name;
	
	public Member(){} // 엔티티는 기본 객체 생성자가 존재해야 함
	
	public Member(String name){
		this.name = name;
	}
	
	public int getSeq() {
		return seq;
	}
	
	public void setSeq(int id) {
		this.seq = id;
	}
	
	public String getName() {
		return name;
	}
	
	public void setName(String name) {
		this.name = name;
	}
	
	@Override
	public String toString() {
		String result = "[member_" + seq + "] " + name;
		return result;
	}
}
```

- 회원 Entity는 그다지 특별한 것은 없다.
- 일반적인 Entity 구성을 따른다.

```java
// Phone Entity
package jpa3;

import javax.persistence.Column;
import javax.persistence.Entity;
import javax.persistence.FetchType;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;
import javax.persistence.JoinColumn;
import javax.persistence.ManyToOne;

@Entity
public class Phone {
	
	@Id
	@Column(name="seq")
	@GeneratedValue(strategy=GenerationType.AUTO)
	private int seq;
	
	@Column(name="no")
	private String no;
	
	@ManyToOne(targetEntity=Member.class, fetch=FetchType.LAZY) // (1)
	@JoinColumn(name="member_id") // (2)
	private Member member;
	
	public Phone() {}
	public Phone(String no){
		this.no = no;
	}
	
	public int getSeq() {
		return seq;
	}
	
	public void setSeq(int seq) {
		this.seq = seq;
	}
	
	public String getNo() {
		return no;
	}
	
	public void setNo(String no) {
		this.no = no;
	}
	
	public Member getMember() {
		return member;
	}
	
	public void setMember(Member member) {
		this.member = member;
	}	
	
	@Override
	public String toString() {
		String result = "[phone_"+seq+"] " + no;
		return result;
	}
	
}
```

- @ManyToOne 어노테이션에서 관계를 설정할 매핑 정보를 넣는다.
- 여기서는 LAZY 전략을 사용한다.
- 즉, 연관관계에 있는 [[엔티티(Entity)]]에 접근할 때, DB에 [[쿼리(query)]]를 날려 [[엔티티(Entity)]]를 조회한다.

- 그리고 Member Entity를 사용한다고 명시적으로 정의를 했다. 
- @JoinColumn 어노테이션이 있는데 여기에 현재 Entity에서 관계할 Entity에 매핑할 키 값을 정의한다.

- 즉, phone 테이블의 member_id 컬럼이 member 테이블 seq로 정의된다.
- member entity에서 seq가 @Id 어노테이션으로 정의가 되어있다.

- Repository는 간단하게 코드만 보고 넘어간다.

```java
// Repository
package jpa3;

import org.springframework.data.jpa.repository.JpaRepository;

public interface MemberRepository extends JpaRepository<Member, Integer>{}

package jpa3;

import org.springframework.data.jpa.repository.JpaRepository;

public interface PhoneRepository extends JpaRepository<Phone, Integer> {}
```

- 실제로 위의 코드를 돌려보는 프로그램을 작성한다.

```java
// Application
package jpa3;

import java.util.List;

import javax.transaction.Transactional;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.CommandLineRunner;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class Jpa3Application implements CommandLineRunner{
	
	@Autowired
	private MemberRepository mr;
	
	@Autowired
	private PhoneRepository pr;
	
	public static void main(String[] args) {
       SpringApplication.run(Jpa3Application.class, args);
	}
	
	@Override
	@Transactional
	public void run(String... args) throws Exception {
		
		Member first = new Member("Jung");
		Member second = new Member("Dong");
		Member third = new Member("Min");
		
		mr.save(first);
		mr.save(second);
		mr.save(third);
		
		Phone p = new Phone("010-XXXX-YYYY");
		p.setMember(first);
		pr.save(p);
		
		List<Phone> phone = pr.findAll();
		
		for( Phone e : phone ){
			System.out.println(e.toString() +" "+ e.getMember().toString());
		}
		
		mr.deleteAll();
		pr.deleteAll();
	}
}

// >> [phone_111] 010-XXXX-YYYY [member_155] Jung
```

- 새로운 회원을 생성한다.
- 그리고 해당 정보를 DB에 저장한다.
- 새로운 핸드폰 하나를 만들고 한 회원의 소유로 정의하고 저장한다. 
- 핸드폰의 전체 목록을 가져와서 하나씩 자신의 정보와 소유주를 출력한다.