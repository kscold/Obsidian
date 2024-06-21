- [[웹(web)]] 브라우저 밖(서버사이드 어플리케이션([[SSR(Server Side Rendering)]]), 데스크톱 애플리케이션)의 자바스크립트를 위한 모듈 생태계의 규칙을 설립하기 위한 프로젝트이다.

- [[모듈(Module)]]의 핵심은 모듈을 만들어서 바깥으로 내보내서 가져다 쓸 수 있는가이다.

- CommonJS의 [[module.exports()]]와 [[require()]]는 [[객체(Object)]]임으로 세부적인 설정을 수정할 수 있다.

- [[리액트(React)]]와 같은 라이브러리에는 [[ES module]]형태의 JS module system이 표준화되면서 Node.js v8.5.0에 도입되었다.


## Common JS의 주요 명세

- 모든 [[모듈(Module)]]은 자신만의 독립적인 실행 영역이 있어야한다.
- [[모듈(Module)]] 정의는 [[전역 객체(Global Object)]]인 [[module.exports()]] [[객체(Object)]]를 이용한다.  

- [[모듈(Module)]]을 사용할지 안할지 분리하는 작업을 위해 exports를 써서 구분한다.

- [[모듈(Module)]] 사용은 [[require()]] 함수를 이용한다.




