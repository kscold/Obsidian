- [[객체(Object)]]는 양방향 참조가 존재하기 때문에 어느 쪽에서 [[외래 키(Foreign Key)]]를 관리할지 정해야 한다.

- [[외래 키(Foreign Key)]]를 가진 [[테이블(Table)]]을 매핑한 엔티티(Entity)에서 [[외래 키(Foreign Key)]]를 관리하는게 효율적이다.
- 즉, 외래 키를 가진 엔티티(Entity)가 주인이라고 생각하면 쉽다.

- [[일대다(OneToMany)]], 다대일 관계에서 항상 '다'쪽이 [[외래 키(Foreign Key)]]를 가진다.
- 즉, 1:N([[일대다(OneToMany)]]), N:1([[다대일(ManyToOne)]]) 경우에 항상 N에 FK([[외래 키(Foreign Key)]])가 있으므로 N 쪽이 연관관계의 주인이다.

- 주인이 아닌 쪽은 [[외래 키(Foreign Key)]]를 변경할 수 없고 읽기만 가능하다.

- 양방향([[다대다(ManyToMany)]])은 FK([[외래 키(Foreign Key)]])가 있는 쪽이 연관관계의 주인이다.


- FK([[외래 키(Foreign Key)]])를 관리할 때 연관관계의 주인만 사용한다.

- 양방향 연관관계는 항상 서로를 참조해야 한다.


## 다대일 단방향 `[N:1]`

![](https://blog.kakaocdn.net/dn/bbOmYO/btrc7OGHP52/9oJ5bUvzDsxPqk5LPg9HB0/img.png)

- 아래 코드는 회원 엔티티 예시이다.

```java
@Entity
public class Member {
    
    @Id 
    @GeneratedValue
    @Column(name = "MEMBER_ID")
    private Long id;
    
    private String username;
    
    @ManyToOne
    @JoinColumn(name = "TEAM_ID")
    private Team team;
}
```

- 아래 코드는 팀 엔티티 예시이다.

```java
@Entity
public class Team {

    @Id @GeneratedValue
    @Column(name = "TEAM_ID")
    private Long id;
    
    private String name;
}
```

- 회원은 Member.team으로 팀 엔티티를 참조 가능하다.
- 팀은 회원 참조 필드 없어서 참조 불가능하다.
- 따라서 회원과 팀은 다대일 단방향 연관관계이다.
- @JoinColumn(name = "TEAM_ID")를 사용해서 Member.team 필드를 TEAM_ID FK와 매핑

## 다대일 양방향 `[N:1, 1:N]`

![](https://blog.kakaocdn.net/dn/d6HJaa/btrc3qGA5Jw/oWRi8keSetysgjwDD8tmQk/img.png)

- 실선이 연관관계의 주인(Member.team)이고 점선(Team.members)은 주인이 아니다. 

```java
// Member 엔티티
@Entity
public class Member {

    @Id @GeneratedValue
    @Column(name = "MEMBER_ID")
    private Long id;
    
    private String username;
    
    @ManyToOne
    @JoinColumn(name = "TEAM_ID")
    private Team team;
    
    public void setTeam(Team team) {
        this.team = team;
        // 무한루프에 빠지지 않도록 체크
        if(!team.getMembers().contains(this)) {
            team.getMembers().add(this)
        }
    }
}
```

```java
// Team 엔티티
@Entity
public class Team {

    @Id @GeneratedValue
    @Column(name = "TEAM_ID")
    private Long id;

    private String name;

    @OneToMany(mappedBy = "team")
    private List<Member> members = new ArrayList<Member>();

    public void addMember(Member member) {
        this.members.add(member);
        // 무한루프에 빠지지 않도록 체크
        if (member.getTeam() != this) {
            member.setTeam(this);
        }
    }
}
```

