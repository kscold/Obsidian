- [[toString()]]를 사용하는 효과와 똑같이 만들어준다.
- 기준은 필드 내의 매개변수를 기준으로 한다.


```java
package com.example.firstproject.dto;  
  
import com.example.firstproject.entity.Article;  
import lombok.AllArgsConstructor;  
import lombok.ToString;  
  
@AllArgsConstructor  
@ToString  
public class ArticleForm {  
    private String title; // 제목을 받을 필드  
    private String content; // 내용을 받을 필드  

	// @ToString이 자동적으로 toString() 메서드를 대체
	/* public String toString() {  
        return "ArticleForm{" +  
                "title='" + title + '\'' +  
                ", content='" + content + '\'' +  
                '}';  
    } */
}
```