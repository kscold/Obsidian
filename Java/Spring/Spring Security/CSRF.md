- CSRF는 Cross-Site Request Forgery의 약어로, 사이트간 요청 위조를 의미한다.
- 공격자가 사용자가 로그인한 웹사이트를 통해 사용자가 모르는 상황에서 원치 않는 동작을 수행하게 하는 보안 취약점이다.

![](https://blog.kakaocdn.net/dn/ykYny/btsbqAWDgFQ/mair3XLK4ink7y5CC2H6ZK/img.png)

- 위 이미지와 같이 공격을 방지하기 위해 웹 개발자들은 요청을 보낼 때 추가적인 보안 검증 절차를 구현하고, 사용자에게 요청의 적절성을 확인하도록 요청하는 등의 대응 방안을 마련해야 한다. 


## CsrfFilter

- CsrfFilter는 웹 어플리케이션에서 CSRF(Cross-Site Request Forgery) 공격을 방지하기 위한 필터다. 

- Spring Security에서 제공하는 CsrfFilter는, [[HTTP]] 요청에 대해 CSRF 토큰을 생성하고, 이 토큰을 쿠키에 저장한다.
- 그리고 나서, 모든 POST, PUT, DELETE 요청에 대해 CSRF 토큰이 함께 전송되어야 하는지를 검증한다.
- 만약 요청에 CSRF 토큰이 없다면, 해당 요청을 차단한다. 


- Spring Security를 사용하는 웹 어플리케이션에서는, CsrfFilter를 설정하여 간단하게 CSRF 공격으로부터 보호할 수 있다.

- http.csrf()로 설정하면 활성화되지만 기본 활성화되어 있으므로 따로 설정할 필요는 없다. 
- 기능이 활성화되면 아래와 같은 역활을 수행한다.

- 모든 요청에 랜덤하게 생성된 토큰을 HTTP 파라미터로 요구한다.
- 요청 시 전달되는 토큰 값과 서버에 저장된 실제 값과 비교한 후 만약 일치하지 않으면 요청은 실패한다.

## 테스트

- 아래와 같이  http.csrf() 를 설정해주지 않은상태에서 테스트를 시작한다.

```java
@Configuration
@EnableWebSecurity
public class SecurityConfig {
	
	@Bean
	public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
		http
			.authorizeRequests()
			.anyRequest()
			.permitAll(); 
		
		http
			.formLogin();
		
		return http.build();
	}
	
}
```

- postman을 사용하여 POST로 Body나 Header에 토큰값을 담지 않고 요청을 보낸다.

![](https://blog.kakaocdn.net/dn/bpU5HL/btsbkGLes1B/axJgKPSLTMhpcp6PxqX5ZK/img.png)

- CsrfFilter.java의 doFilterInternal() 에서 (1) 은 서버에 저장된 토큰이고 (2) 는 클라이언트에서 주는 토큰인데 토큰을 주지 않았기에 null로 확인이 된다.

![](https://blog.kakaocdn.net/dn/bPRrFB/btsbn6IABqT/3M55UMLiZLC9AMBojN8yKk/img.png)

![](https://blog.kakaocdn.net/dn/1VM5f/btsbmXrA3wc/QLLqkwttwMnloiu16tgCQk/img.png)

![](https://blog.kakaocdn.net/dn/SqzE8/btsbmPfZxdK/PZsOL6OFtq2CXAhqCqj8N0/img.png)

- 토큰 값을 헤더 키 :  X-CSRF-TOKEN  값 : 355391cd-13a2-4ddd-9c96-b49fb21f8a23 를 넣어 다시 요청을 보낸다.

![](https://blog.kakaocdn.net/dn/wF2WF/btsbmUn6XmL/mHD8ZpAtWkrvkTV4C3ru7k/img.png)

- 토큰과 함께 요청를 했으니 actualToken에 토큰값이 확인이 가능하다.

![](https://blog.kakaocdn.net/dn/co8dDv/btsbl9yZmwC/oH8DVKBIB8K1PkrGNJiWK0/img.png)

- 비활성화는 아래 방법으로 가능하다.

```java
http.csrf().disable();
```