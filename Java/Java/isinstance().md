- [[instanceof]]과 isInstance() 모두 [[객체(Object)]]의 [[클래스(Class)]]를 확인하는데 사용한다.
- 하지만 클래스의 체크 시점이 정적(컴파일 타임)이냐 동적(런타임)이냐의 차이를 보인다.

- 즉, instanceof는 컴파일 타임에 타입 에러를 체크하고, isInstance()는 런 타임에 타입 에러 체크를 합니다.