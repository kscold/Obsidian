
@ModelAttribute [[어노테이션(Annotation)]]
[[Controller]]의 [[메소드(Method)]]는 Model이라는 타입의 [[객체(Object)]]를 파라미터로 받을 수 있다.

@ModelAttribute는 Spring MVC에서 Controller의 메소드 파라미터가 자동으로 모델에 추가되도록 하는 어노테이션

강제로 전달받은 파라미터를 Model에 담아서 전달하도록 할 때 필요한 어노테이션

스프링에서 Java beans 규칙(Getter, Setter, 생성자 포함)에 맞는 객체는 파라미터 전달이 자동으로 가능.

하지만 일반 변수의 경우, 자동 전달 불가능. model 객체를 통해서 전달 필요.