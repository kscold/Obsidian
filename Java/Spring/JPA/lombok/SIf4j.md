- [[롬복(lombok)]]에서 @[[@SIf4j]]를 사용하여 간평하게 [[로깅(logging)]]을 할 수 있지만
- build.gradle에 implementation 'ch.qos.logback:logback-classic:1.2.3'를 추가하면 사용할 수 있다.

## 문법

```java
private final Logger logger = LoggerFactory.getLogger(this.getClass());
```

### static으로 선언한 이유 

- [[static]]을 선언하면 [[클래스 변수(class variable)]]로 객체 생성이 될 때마다 해당 객체를 매번 생성하지 않고 초기 [[클래스(Class)]] 로딩 시 한 번만 생성해서 사용하게 된다.

- 그러나, Spring에서는 [[객체(Object)]]를 굳이 [[싱글톤(Singleton)]] 형태로 디자인하지 않아도 객체를 [[싱글톤(Singleton)]]과 같이 한 번만 생성해서 사용하게 된다. 

- 따라서 무조건적인 static을 선언해 Perm 영역의 공간을 소비하지는 말자.
- Perm(Permanent Generation) 영역의 약자로 [[객체(Object)]]의 생명주기가 영구적일 것으로 생각되는 객체를 관리한다.

- 또한, 직렬화하는 것을 피할 수 있다. 

### final으로 선언한 이유

- 로그를 찍는 경우 Logger는 초기 생성된 이후 변경될 필요가 없다. 
- 따라서 [[final]]로 선언하여 유지보수와 가독성을 높이기 위함이다. 

### LOGGER라는 대문자로 변수 선언한 이유

 - Java에서 상수 선언시 통상적으로 대문자를 많이 사용한다.
 - static final의 멤버 변수의 경우 상수 선언 시 많이 사용되고, 따라서 관행상 대문자로 변수명을 짓게 된 것이다. 

### 기능

- Console 창에 해당 로그가 찍힌다.
- 따라서 프로그램이 오류 발생 시 어디서 어떤 이유로 오류가 발생하는지 알 수 있어 이슈 처리가 용이하다. 

## 예시

- 예를 들어 게시판 프로젝트를 만들 때 아래와 같이 [[클래스(Class)]] 안에[[메서드(Method)]]에 SIf4j의  Looger기능을 사용할 수 있따.

```java
private static final Logger LOGGER = LoggerFactory.getLogger(BoardCtr.class);

	/*
	* 게시판 트리 ajax
	* @param response
	*/
	
    @RequestMapping("/boardListByAjax")
    @ResponseBody
    public void boardListByAjax(HttpServletResponse response){
        List<?> listview = boardGroupSvc.selectBoardGroupList();
        
        TreeMaker tm = new TreeMaker();
        String treeStr = tm.makeTreeByHierarchy(listview);
        
        response.setContentType("application/json;charset=UTF-8");
        
        try {
            response.getWriter().print(treeStr);
        }catch (IOException ex){
            LOGGER.error("boardListByAjax");
        };
        
	}
```