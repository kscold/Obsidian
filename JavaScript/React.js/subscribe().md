- 구독 또한 [[스토어(Store)]]의 내장함수 중 하나이다. 
- subscribe 함수는, 함수 형태의 값을 매개변수로 받아온다. 
- subscribe 함수에 특정 함수를 전달해주면, [[액션(Action)]]이 [[dispatch()]] 되었을 때 마다 전달해준 함수가 호출된다.

- 리액트에서 [[리덕스(Redux)]]를 사용하게 될 때 보통 이 함수를 직접 사용하는 일은 별로 없다.
- 그 대신에 react-redux 라는 라이브러리에서 제공하는 connect() 또는 useSelector() [[Hooks]] 을 사용하여 [[리덕스(Redux)]] [[스토어(Store)]]의 상태([[state]])에 구독한다.