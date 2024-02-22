- [[toString()]]를 사용하는 효과와 똑같이 만들어준다.
- 기준은 필드 내의 매개변수를 기준으로 한다.

- @ToString.Exclude [[어노테이션(Annotation)]]으로 설정하여 [[toString()]] 메서드를 안만들고 싶은 것을 선택할 수 있다.
- 즉, 순환 참조 [[외래 키(Foreign Key)]]로 들어와 있는 [[엔티티(Entity)]]의 [[toString()]]을 막을 수 있다.

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