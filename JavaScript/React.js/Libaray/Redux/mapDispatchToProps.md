- [[리덕스(Redux)]]에서  사용되는 [[connect()]]의 두 번째 인자로 들어가는 [[함수(Function)]]이다.

- [[action]]을 [[Reducer]] [[함수(Function)]]에게 보내는 역할을 가진 [[dispatch()]]를 [[props]]로 보낼 수 있다.


## 문법

```jsx
function mapDispatchToProp(dispatch, ownProps?)
```


### dispatch

- [[리덕스(Redux)]]의 [[스토어(Store)]].[[dispatch()]]와 같다.
### ownProps

- 생략가능하다.
- [[컴포넌트(Component)]]가 현재 가지고 있는 모든 [[props]]를 보여준다.


## 예시

![](https://velog.velcdn.com/images%2Fiamhayoung%2Fpost%2Fd2636db7-fa60-419d-9a26-606e73d74125%2Fimage.png)

- mapDispatchToProps의 첫번째 인자의 [[dispatch()]]를 그대로 return해주고,Home [[컴포넌트(Component)]]의 [[props]]으로 받은 [[dispatch()]]를 보면 store.[[dispatch()]]와 같은 [[메서드(Method)]]드가 들어있다는 것을 알 수 있다.

- 이렇게 mapDispatchToProp을 이용함으로써 [[컴포넌트(Component)]] 내에서 [[dispatch()]]를 사용할 수 있게 되었다.