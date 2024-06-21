- 자바스크립트의 엄격 모드(strict mode)는 자바스크립트의 제한된 버전을 선택하여 암묵적인 느슨한 모드(slopppy mode)를 해제하기 위한 방법이다.

- 따라서 엄격모드를 사용하면 기존에 조용히 무시되던 에러들을 일으킨다.
- 자바스크립트 엔지의 최적화 작업을 어렵게 만드는 실수를 바로잡고 가끔씩 느슨한 모드에 비해 더 빨리 작동하도록 만든다.


## 바닐라 자바스크립트에서 사용

1. 바닐라 자바스크립트에서는 "use strict" 지시어를 자바스크립트 파일에 사용하면 사용할 수 있다.
2. 특정 [[함수(Function)]] 안에서만 "use strict" 지시어를 사용해 그 [[함수(Function)]]만 strict mode를 사용할 수 있다.
3. [[HTML(Hyper Text Markup Language)]] `<script>` 속성에 type="module"를 주어 사용할 수 있다.
4. [[클래스(Class)]] [[키워드(Keyword)]]를 주면 사용이 자동적으로 strict mode가 사용 가능하다.


## [[리액트(React)]]

- [[index.js]]에 `<React.StrictMode>`에서 삭제하거나 추가할 수 있다.
- [[리액트(React)]] 프로젝트에서 [[리액트(React)]]의 레거시 기능들을 사용하지 못하게 하는 기능이다. 

- 이를 사용 하면 문자열, [[ref]], componentWillMout 등 나중에는 완전히 사라질 옛날 기능을 사용했을 때 경고를 띄워준다.
