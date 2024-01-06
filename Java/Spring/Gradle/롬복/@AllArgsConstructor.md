- [[롬복(lombok)]]의 기능으로 이 [[어노테이션(Annotation)]]을 사용하면 클래스 안쪽의 모든 [[Java/필드(Field)]], 즉, 매개변수로 하는 생성자가 자동으로 만들어진다.


### 예시
```java
package com.example.firstproject.dto;  
  
import com.example.firstproject.entity.Article;  
import lombok.AllArgsConstructor;  
  
@AllArgsConstructor  
public class ArticleForm {  
    private String title; // 제목을 받을 필드  
    private String content; // 내용을 받을 필드  
  
  
    // 롬복의 @AllArgsConstructor 사용하면 밑의 생성자 코드를 대체할 수 있음
    /* public ArticleForm(String title, String content) {      
	    this.title = title;     
	    this.content = content;    
	} */  
    
}
```