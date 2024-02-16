- 클라이언트가 데이터를 입력하거나 클라이언트의 정보에 대한 요청 값을 가지고 있는 [[클래스(Class)]]이다.

|  | name에 할당된 값을 반환하며 지정된 파라미터 값이 없으면 null 값을 반환 |
| ---- | ---- |
| String[] getParameterValues(name) | name의 모든 값을 String 배열로 반환 |
| Enumeration getParameterNames() | 요청에 사용된 모든 파라미터 이름을 java.util.Enumeration 타입으로 반환 |
| void setCharacterEncoding(env) | post방식으로 요청된 문자열의 character encoding을 설정

## 메서드

### [[getParameter()]]

- [[HTTP]] Request 요청을 쿼리스트링을 가져오는 [[메서드(Method)]]이다.