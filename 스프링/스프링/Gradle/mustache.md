- mustache는 뷰 템플릿을 만드는 도구이다.
- 뷰 템플릿 엔진을 의미한다.
- 확장자는 mushache를 사용한다.
- 약자 doc Tab 키를 이용하여 vscode의 !와 비슷하게 자동완성 시킬 수 있다.
- 머스테치의 기본 위치는 src/main/resources/templates로 이 위치에 머스테치 파일을 저장하면 스프링 부트에서 자동으로 로딩한다.


### GetMapping로 연동
자바 메소드에 @Controller 애노테이션이 선언 되어 있고 GetMapping과 같은 애노테이션이 있으면 return 값에 자동적으로 .mushache 확장자를 가진 머스테치 파일이 연동된다. 그러나 이 과정만 해서는 404 오류가 나는데, 이때 Model 타입의 model 매개변수를 추가한다.