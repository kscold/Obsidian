- Context는 [[리액트(React)]] [[컴포넌트(Component)]]간에 어떠한 값을 공유할수 있게 해주는 기능이다.

- 주로 Context는 전역적(global)으로 필요한 값을 다룰 때 사용하나, 꼭 전역적일 필요는 없다.
- 따라서 Context API는 [[Props Drilling]]을 방지할 수 있다.


## [[리액트(React)]] context를 사용하는 4단계

1. [[createContext()]] [[메서드(Method)]]를 사용해 context를 생성한다.
2. 생성된 context를 가지고 [[Context.Provider]]로 [[컴포넌트(Component)]] 트리를 감싼다.
3. [[value]] [[props]]을 사용해 [[Context.Provider]]에 원하는 값을 입력한다.
4. [[Context.Consumer]]와 [[useContext()]] 통해 필요한 [[컴포넌트(Component)]]에서 그 값을 불러온다.



