json을 뱉는 방식으로 [[@ResponseBody]] 어노테이션을 선행적으로 이용해 필요한 값으로만 json 형식 파일을 만든다.


예시)
  
[[@GetMapping]]("hello-api") // hello-api라는 라우팅을 생성

@ResponseBody // 로직이 적용된 데이터 값만 반환

public Hello helloApi(@RequestParam("name") String name){ // url인 hello-api?name=사용자입력 을 사용할 수 있는 함수 생성

    Hello hello = new Hello(); // 이렇게 함수를 하나의 새로운 객체 new Hello()로 선언, 이후 Hello 객체 타입으로 선어한 hello가 인스턴스가 됨

    hello.setName(name);

    return hello;

}

  

static class Hello {

    private String name;

  

    public String getName() {

        return name;

    }

  

    public void setName(String name) {

        this.name = name;

    }

}