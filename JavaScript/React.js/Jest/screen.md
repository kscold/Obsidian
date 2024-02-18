- [[Jest]]에서 작동하는 @testing-library/react라이브러리의 기능으로 Jest의 [[React.js/Jest/render()|render()]] 함수로 컴포넌트를 Jest로 가져오면 screen을 통해 접근을 할 수 있다.

- screen의 정의되어 있는 메서드 getByRole을 통해서 [[HTML]] 태그의 attribute 중 [[role]]를 설정함으로써 버튼 객체와 같은 요소에 접근할 수 있다.


## getByRole()

-  [[HTML]] 태그의 attribute 중 [[role]]를 설정하고 screen.getByRole()의 매개변수에 문자열로 값을 넣어 [[Jest]]로 테스트 코드를 돌린다.

## toHaveTextContent()

- [[expect()]]의 toHaveTextContent()를 통해서 [[이벤트(event)]]의 결과값을 예상한다.