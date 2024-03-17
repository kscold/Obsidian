- 스프링 시큐리티([[Spring Security]])에서 권한들과 역할은 기본적으로 GrantedAuthority에 저장된다.

- GrantedAuthority는 권한이나 역할의 이름을 반환하는 [[메서드(Method)]]를 제공한다.

## GrantedAuthority [[인터페이스(Interface)]]

```java
public interface GrantedAuthority extends Serializable {

	String getAuthority();

}
```

## SimpleGrantedAuthority

- SimpleGrantedAuthority [[클래스(Class)]]는 GrantedAuthority [[인터페이스(Interface)]]의 기본으로 구현되는 객체입니다.

```java
public final class SimpleGrantedAuthority implements GrantedAuthority {
	
	private static final long serialVersionUID = SpringSecurityCoreVersion.SERIAL_VERSION_UID;
	
	private final String role;
	
	public SimpleGrantedAuthority(String role) {
		Assert.hasText(role, "A granted authority textual representation is required");
		this.role = role;
	}
	
	@Override
	public String getAuthority() {
		return this.role;
	}
    
    ...
}
```