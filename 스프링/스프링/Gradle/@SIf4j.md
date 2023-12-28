- [[로깅(logging)]]을 할 수 있게 만들어 주는 [[어노테이션(Annotation)]]이다.

### 문법
- 어노테이션을 선언한 후 원하는 위치에서 log.info(클래스.toString())으로 부르면 된다.

```java
package com.example.firstproject.controller;  
  
import com.example.firstproject.dto.ArticleForm;  
import com.example.firstproject.entity.Article;  
import com.example.firstproject.repository.ArticleRepository;  
import lombok.extern.slf4j.Slf4j;  
import org.springframework.beans.factory.annotation.Autowired;  
import org.springframework.stereotype.Controller;  
import org.springframework.web.bind.annotation.GetMapping;  
import org.springframework.web.bind.annotation.PostMapping;  
  
@Slf4j  
@Controller  
public class ArticleController {  
    @Autowired // 스프링 부트가 미리 생성해 놓은 리파지터리 객체 주입(DI)  
    private ArticleRepository articleRepository;  
  
    @GetMapping("/articles/new")  
    public String newArticleForm() {  
        return "articles/new";  
    }  
  
    @PostMapping("/articles/create")  
    public String createArticle(ArticleForm form) {  
        System.out.println(form.toString());  
  
        // 1. DTO를 엔티티로 변환  
        Article article = form.toEntity();  
        log.info(article.toString()); // 롬복의 로깅을 호출
        // System.out.println(article.toString()); // DTO가 엔티티로 잘 변환되는지 확인 출력  
  
        // 2. 리파지터리로 엔티티를 DB에 저장  
        Article saved = articleRepository.save(article);  
        log.info(saved.toString()); // 롬복의 로깅을 호출
        // System.out.println(saved); // article이 DB에 잘 저장되는지 확인 출력  
  
        return "";  
    }  
}
```