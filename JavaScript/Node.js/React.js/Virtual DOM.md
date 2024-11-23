- [[DOM(Document Object Model)]]를 추상화한 자바스크립트 [[객체(Object)]]이며, 실제 DOM의 사본에 가깝다.
- 따라서 Virtual DOM은 실제 [[DOM(Document Object Model)]]을 메모리에 복사한 것이다.

- [[리액트(React)]]는 대규모 [[SPA(Single Page Application)]]와 다이나믹 UI의 웹 페이지를 만들기 위해서 존재하며, 만약 규모가 작고 정적인 이전의 웹 애플리케이션이라면 일반 DOM이 성능이 더 좋다. (상황에 따라 어느 쪽이 좋은지 다를 수 있다는 것이다.)


## [[리액트(React)]]에서 동작과정

1. 데이터를 업데이트하면 전체 UI를 Virtual DOM에 리렌더링한다.
![](https://blog.kakaocdn.net/dn/ZWUVL/btrqIsDpGw0/MHm6awjZ4yb3lFdPwgMMw1/img.png)

2. 이전 Virtual DOM에 있던 내용과 현재 내용을 비교한다. (Diffing한다고 부른다.)

![](https://blog.kakaocdn.net/dn/eDYS8l/btrqFkNpiqH/mzedSHhNlO61c5nb2HMKwk/img.png)

3. 바뀐 부분만 실제 [[DOM(Document Object Model)]]에 적용한다. (재조정(reconciliation)이라고 한다.)

![](https://blog.kakaocdn.net/dn/OXLn4/btrqIsckSo4/aP37ktXfhVVINPg6hbiCeK/img.png)


![[Pasted image 20231218213654.png]]

- 리액트 문법은 리액트 [[컴포넌트(Component)]]에서 요소 여러개를 왜 하나의 요소로 감싸 주어야하는 이유도 여기서 찾아볼 수 있다.

- Virtual DOM에서 [[컴포넌트(Component)]]의 변화를 감지해 낼 때 효율적으로 비교할 수 있도록 [[컴포넌트(Component)]] 내부는 하나의 [[DOM(Document Object Model)]] 트리 구조로 이루어져야 한다는 규칙이 있기 때문이다.
