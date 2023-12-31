
- [[객체(Object)]]로 문서 구조를 표현하는 방법으로 XML이나 HTML로 작성한다.
- 따라서 문서 객체한 HTML, head, body와 같은 태그들을 Javascript가 이용할 수 있는 객체를 의미한다.
- DOM은 트리 형태라서 특정 노드르 찾거나 수정, 제거, 원하는 위치에 삽입이 가능하다.



![[Pasted image 20240101213717.png]]

- DOM이 존재하기 때문에 Javascript는 HTML 태그들을 수정할 수 있는 것이다.




### DOM의 문제점

- DOM API를 사용하는 서비스들은 동적 UI에 최적화되어 있지 않다는 문제가 있다. HTML 자체는 정적이므로 자바스크립트를 이용해 동적으로 만들 수 있다.

- 그러나 페이스북을 예시로만 들어도 div의 갯수가 천문학적으로 커지기 때문에 규모가 큰 웹이 DOM에 직접 접근하여 변화를 주다 보면 성능이슈가 발생한다.
- DOM 자체는 빠르지만 DOM에 변화가 일어나면 웹 브라우저가 CSS를 다시 연산하고, 레이아웃을 구성하기 때문에 이 과정에서 시간이 허비가 된다.

- 따라서 리액트는 [[Virtual DOM]]을 사용하여 실제 DOM에 접근하여 조작하는 대신 이를 추상화한 자바스크립트 객체를 구성하여 사용한다.