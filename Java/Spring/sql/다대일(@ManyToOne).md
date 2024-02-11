## 다대일 단방향 [N:1]

![](https://blog.kakaocdn.net/dn/bbOmYO/btrc7OGHP52/9oJ5bUvzDsxPqk5LPg9HB0/img.png)

- 아래 코드는 회원 엔티티 예시이다.

```java
@Entity
public class Member {
    
    @Id @GeneratedValue
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

### **다대일 양방향 [N:1, 1:N]**

![](https://blog.kakaocdn.net/dn/d6HJaa/btrc3qGA5Jw/oWRi8keSetysgjwDD8tmQk/img.png)

실선이 연관관계의 주인(Member.team)이고 점선(Team.members)은 주인이 아니다. 

**회원 엔티티**

```
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

**팀 엔티티**

```
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

- 양방향은 FK가 있는 쪽이 연관관계의 주인
- 1:N, N:1 경우에 항상 N에 FK가 있으므로 N 쪽이 연관관계의 주인
- JPA는 FK를 관리할 때 연관관계의 주인만 사용
- 양방향 연관관계는 항상 서로를 참조해야 함

출처: [https://mjmjmj98.tistory.com/152](https://mjmjmj98.tistory.com/152) [👾:티스토리]