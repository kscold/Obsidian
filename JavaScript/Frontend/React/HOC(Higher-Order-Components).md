- HOC(**H**igher-**O**rder-**C**omponents)은 컴포넌트를 개발하는 하나의 패턴으로, [[컴포넌트(Component)]]를 인자로 받아 새로운 [[컴포넌트(Component)]]로 변환해 반환하는 [[함수(Function)]]이다.

- HOC은 같은 로직을 다수의 컴포넌트에 동일 적용해야할 때 굉장히 유용하게 사용할 수 있다.

- [[함수형 컴포넌트(Functional Component)]]가 생기고 hook이 도입된 이후, HOC을 사용하는 경우는 많이 줄어들고 있다.

- HOC이 보통 [[클래스형 컴포넌트(Class Component)]]에서 [[리액트(React)]] [[생명 주기(Life Cycle)]]을 고려한 재사용 가능한 로직을 만들기 위해 사용되기 때문인데, [[함수형 컴포넌트(Functional Component)]]에서는 거의 대부분 [[Hooks]]으로 대체가 가능하다.

- 예를 들어, 우리가 프로젝트의 상태를 하나의 [[스토어(Store)]]에 저장해놓고 사용한다고 했을 때, 컴포넌트에서 store에 접근해 상태값을 꺼내오기 위해서는 상태값을 사용하는 컴포넌트마다 store와 연결시켜주는 코드가 필요하다.
- 컴포넌트 내부에 매번 작성하기보다 애초에 store와 연결되어있는 컴포넌트를 만들면 좋지 않을까라고 생각이 들 수 있는데 이 부분이 대표적인 HOC, [[리덕스(Redux)]]의 [[connect()]]이다.
- 대표적 HOC으로 소개된 [[connect()]]만 보아해도, [[useSelector()]]와 [[useDispatch()]]를 사용하는게 더 코드를 직관적이고 간편하게 만든다.


## 예시

- HOC의 이해를 위해서 아래와 같이간단한 HOC을 만든다.

- 컴포넌트 렌더 시에 로딩 상태값을 두고, 로딩이 되지 않았다면 로딩 메세지를 보여주고 로딩이 끝났다면 컨텐츠가 담긴 컴포넌트를 보여주고 싶다.

- 여러개의 컴포넌트에 해당 로직을 적용하고 싶으니 해당 로직을 공통으로 가지고 있는 HOC을 만든다.
- HOC은 주로 with를 prefix로 지정해 짓기 때문에 withLoading이라는 HOC을 만들었다.

```jsx
import React from 'react';
import Loading from '../components/Loading';

const withLoading = (WrappedComponent) => props => {
	if (props.isLoading) return <Loading/>
	return <WrappedComponent {...props}/>
}

export default withLoading;
```

- 만든 HOC을 사용해본다.

- 핵심적인 내용이니 다시한번 강조하자면, HOC은 컴포넌트를 인자로 받아 새로운 [[컴포넌트(Component)]]로 변환해 반환하는 [[함수(Function)]]이다.
- [[함수(Function)]]를 사용하듯, HOC을 사용하기 위해서는 변환하고자 하는 컴포넌트를 인자로 넘겨주면 된다.

```jsx
import React from 'react';
import withLoading from '../hoc/withLoading'

const ComponentA = props => {
	...
}

export default withLoading(ComponentA);
```