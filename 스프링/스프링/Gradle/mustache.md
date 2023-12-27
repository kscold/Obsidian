- mustache는 뷰 템플릿을 만드는 도구이다.
- 뷰 템플릿 엔진을 의미한다.
- 확장자는 mushache를 사용한다.
- 약자 doc Tab 키를 이용하여 vscode의 !와 비슷하게 자동완성 시킬 수 있다.
- 머스테치의 기본 위치는 src/main/resources/templates로 이 위치에 머스테치 파일을 저장하면 스프링 부트에서 자동으로 로딩한다.


### mustache로 만든 페이지의 흐름

1. 자바 [[메서드(Method)]]에 @Controller 애노테이션이 선언 되어 있다.
2. 클라이언트로부터 [[@GetMapping]]을 통해 requset를 받는다.
3. URI 요청을 받음과 동시의 같은 GetMapping에 귀속된 메서드를 수행한다.
4. 뷰 템플릿 페이지에서 사용할 변수를 등록하기 위해 모델([[Model]]) 객체를 매개변수로 가져온다.
5. 모델에서 사용할 변수를 등록한다. (변수 값에 따라 서로 다른 뷰 템플릿이 생성된다.)
6. 메서드를 수행한 결과로 이름.mustache 파일을 반환한다.(이때 return 문에는 파일 이름만 작성하면 된다.)

- 참고 @GetMapping 애노테이션이 있으면 서버가 자동적으로 src/main/resources/templates로 위치에 머스테치 파일을 return 값에 자동적으로 연동시킨다. 따라서 이름.mustache 파일에  404 오류를 피하기 위해서는 model.[[addAttribute]]를 통해 mustache에 올바른 변수 값을 넘기면 된다.

```java
package com.example.firstproject.controller;  
  
import org.springframework.stereotype.Controller;  
import org.springframework.ui.Model;  
import org.springframework.web.bind.annotation.GetMapping;  
  
@Controller  
public class FirstController {  
  
    @GetMapping("/hi")  
    public String niceToMeetYou(Model model) { // model 객체 받아 오기  
        // model 객체가 "승찬" 값을 "username"에 연결해 웹 브라우저로 보냄  
        model.addAttribute("username", "홍");  
        return "greetings"; // greetings.mustache 파일 반환  
    }  
  
  
}
```