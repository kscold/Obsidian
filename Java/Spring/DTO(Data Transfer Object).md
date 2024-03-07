- DTO는 [[HTTP]] 요청 또는 응답의 body에 데이터를 답아서 전송하는데 사용된다.
- [[<form>]] 태그에 실어 보낸 데이터는 서버의 [[컨트롤러(Controller)]](컨트롤러)가 [[객체(Object)]]에 담아 받는데 이 객체를 DTO라고 한다.



![[Pasted image 20240229173714.png]]
## DTO의 실행과정

1. 뷰 페이지(.[[mustache]])를 만들고 [[<form>]]태그의 action 속성으로 데이터를 어디로 보낼지(action="경로"), method 속성으로 데이터를 어떻게 보낼지(method="post") 정의한다.
2. 컨트롤러(이름Controller.java)를 만들고 [[@PostMapping]] 방식으로 URI 주소를 연결한다.
3. 전송받은 데이터를 담아 둘 객체인 DTO(이름.java)를 만든다.
4. 컨트롤러에서 폼 데이터를 전송받아 DTO에 담는다. 




## [[DAO(Data Access Object)]]와 DTO(Data Transfer Object)의 차이

### [[DAO(Data Access Object)]]

- [[리파지터리(Repository)]] package이다.

-  실제로 [[데이터베이스(DataBase)]]에 접근하는 [[객체(Object)]]이다.

- Persistence Layer( = DB에 data를 CRUD하는 계층)이다.
- [[서비스(Service)]]와 DB를 연결하는 고리의 역할을 한다.

- [[SQL]]을 사용(사용자가 직접 코딩)하여 DB에 접근한 후 적절한 CRUD API를 제공한다.
- [[JPA(Java Persistence API)]]는 대부분의 기본적인 CRUD method를 제공하고 있다.

```java
public interface QuestionRepository extends CrudRepository<Question, Long> {
}
```

### DTO(Data Transfer Object)

- DTO package이다.
- 계층간 데이터 교환을 위한 [[객체(Object)]](Java [[Bean]])이다.

- [[데이터베이스(DataBase)]]에서 데이터를 얻어 [[서비스(Service)]]나 [[컨트롤러(Controller)]] 등으로 보낼 때 사용하는 객체이다.

- 즉, DB의 데이터가 Presentation Logic Tier로 넘어오게 될 때는 DTO의 모습으로 바뀌어서 오고가는 것이다.

- 로직을 갖고 있지 않은 순수한 데이터 객체이며, [[Getter and Setter]] 메서드만을 갖는다.
- 하지만 DB에서 꺼낸 값을 임의로 변경할 필요가 없기 때문에 DTO클래스에는 setter가 없다.
- 대신 [[생성자(constructor)]]에서 값을 할당한다.
- 즉 [[@Builder]]

-  Request와 Response용 DTO는 [[뷰(View)]]를 위한 클래스이다.

- 자주 변경이 필요한 [[클래스(Class)]]이다.

- Presentation Model이다.
- toEntity() [[메서드(Method)]]를 통해서 DTO에서 필요한 부분을 이용하여 [[엔티티(Entity)]]로 만든다.

- 또한 Controller Layer에서 Response DTO 형태로 Client에 전달한다.

#### VO(Value Object) 클래스 vs DTO

- VO는 DTO와 동일한 개념이지만 read only 속성을 갖는다.  
- VO는 특정한 비즈니스 값을 담는 객체이고, DTO는 Layer간의 통신 용도로 오고가는 객체를 말한다.