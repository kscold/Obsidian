

## 권한(Authority)와 역할(Role)의 차이점

- 권한과 역할의 차이점은 접근 권한을 얼마나 쪼개는 가에 있다.

- 접근 권한을 개별의 특정 행위로 규정할 때 Authority를 사용한다.
- 접근 권한을 특정 집단으로 규정할 때 Role을 사용한다.
- 쉽게 말해, 접근 권한을 짧게 쪼갤 때 Authority를 사용하며, 길게 쪼갤 때 Role을 사용한다.

- 예를 들어 쇼핑몰 사이트가 있을 때 구매 권한, 판매 권한, 좋아요 권한 등 짧게 쪼갤 때 Authority를 사용한다.
- 반면, 관리자, 구매자, 판매자와 같이 특정 집단으로 묶어 관리할 때에는 Role을 사용한다.

- 접근 권한을 일일히 지정하는 것은 번거롭기 때문에 주로 역할을 사용한다.

- 여기서 [[데이터베이스(DataBase)]]에 회원의 역할을 정의할 때 유의할 사항이 있다. 
- 역할을 정의할 때에는 접두사로 ROLE_로 시작해야 한다.

- 가령 ROLE_USER, ROLE_VENDER, ROLE_ADMIN를 예로 들 수 있다.

```java
/**
 * Specifies a user requires a role.
 * @param role the role that should be required which is prepended with ROLE_
 * automatically (i.e. USER, ADMIN, etc). It should not start with ROLE_
 * @return {@link AuthorizationManagerRequestMatcherRegistry} for further
 * customizations
 */
public AuthorizationManagerRequestMatcherRegistry hasRole(String role) {
	return access(withRoleHierarchy(AuthorityAuthorizationManager.hasRole(role)));
}
```

- [[데이터베이스(DataBase)]]에 역할을 저장할 때, ROLE_로 시작해야 한다.
- 하지만, 주석을 보면 2 - 3 번째 줄에서 ROLE_ 접두사는 자동으로 붙기 때문에 매개변수에 역할만 넣으라고 되어 있다.

- 그리고 구성 정보를 설정해서 역할에 따라 들어갈 수 있는 URL들을 설정할 수 있다.
- 역할 설정에는 hasRole(), hasAnyRole() [[메서드(Method)]]가 있습니다.

```java
.authorizeHttpRequests((requests) -> requests
    .requestMatchers("/myShop").hasRole("VENDOR")
    .requestMatchers("/myHOME").hasAnyRole("USER", "ADMIN")
    .requestMatchers("/myCart").hasRole("USER")
    .requestMatchers("/myShop", "/myHome", "/myCart").authenticated()
    .requestMatchers("/register").permitAll())
```

- 이렇게 [[메서드(Method)]]의 매개변수로 ROLE_역할이 아닌, 역할만 넣는다면 자동으로 ROLE_역할로 인식한다.