- 자바 객체를 [[JSON(Java Script Object Notation)]]으로 바꾸는 작업은 GSON 이라는 패키지를 이용하면 쉽게 할 수 있다.  
- GSON은 구글에서 만든 자바 오브젝트 직렬화/역직렬화 라이브러리다.

- 자바 [[클래스(Class)]]를 다른 곳으로 전송할 때 사용할 수 있는 직렬화(Serialize) 포맷으로 JSON이나 XML 같은 텍스트 포맷을 사용하는 경우가 많다.

- 즉, GSON은 자바 [[객체(Object)]]를 [[JSON(Java Script Object Notation)]]으로, [[JSON(Java Script Object Notation)]]을 자바 [[객체(Object)]]로 쉽게 표현 및 변환이 가능하다.


## 설정

### Maven 설정

```
<dependency>
	<groupId>com.google.code.gson<groupId>
	<artifactId>gson</artifactId>
	<version>2.8.0</version>
</dependency>
```

### Gradle 설정

```
dependencies {
	implementation 'com.google.code.gson:gson:2.8.6'
}
```


## Gson 직렬화

- 흔히 Spring Boot를 이용하여 리스트([[List]])를 [[서비스(Service)]]로 부터 받아와 [[컨트롤러(Controller)]]에서 [[뷰(View)]]로 리스트([[List]])를 [[JSON(Java Script Object Notation)]] 형식으로 전달할 때 사용하는 방식이다.

```java
@RequestMapping(value = "test", method=RequestMethod.GET)
public void gsonTest(Model model) {
    Gson gson = new Gson;
    String getList = testService.getList();
    String json = gson.toJson(getList);

    model.addAttributes("list", list);
}
```

- 이런식으로 [[JSON(Java Script Object Notation)]]으로 직렬화 시킨 데이터를 [[모델(Model)]]에 담아 보내게 되면 [[뷰(View)]]에서 [[JSON(Java Script Object Notation)]]데이터로 작업을 할 수 있게 된다.

## Gson 역직렬화

```java
String jsonInput = "json 데이터";
Example ex = new Gson().fromJson(jsonInput, Example.class); 
```

- 역직렬화는 이런식으로 사용한다고 한다.
## 예시

```java
import com.google.gson.JsonArray;
import com.google.gson.JsonObject;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.ResponseBody;

@Controller
public class GsonTestController {
	
    @ResponseBody
    @GetMapping("/test2")
    public String test2() {
        JsonObject jsonObj = new JsonObject(); // JsonObject 객체 생성
		
        jsonObj.addProperty("title", "GsonTest Title");
        jsonObj.addProperty("content", "GsonTest Content");
		
        JsonArray jsonArr = new JsonArray();
        for (int i = 0; i < 5; i++) {
            JsonObject jsonObj2 = new JsonObject();
            jsonObj2.addProperty("data", i);
            jsonArr.add(jsonObj2);
        }
		
        jsonObj.add("testData", jsonArr);
		
        return jsonObj.toString();
    }
}

// >> {"title":"GsonTest Title","content":"GsonTest Content","testData":[{"data":0},{"data":1},{"data":2},{"data":3},{"data":4}]}
```

```json
{
  "title": "GsonTest Title",
  "content": "GsonTest Content",
  "testData": [
	{
      "data": 0
    },
    {
      "data": 1
    },
    {
      "data": 2
    },
    {
      "data": 3
    },
    {
      "data": 4
    }
  ]
}
```

- 이런식으로 GSON을 사용하여 자바 객체를 JSON으로 손쉽게 이용, 변환이 가능하다.

