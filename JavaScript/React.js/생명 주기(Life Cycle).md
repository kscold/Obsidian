- [[리액트(React)]]에서 생명주기란, [[컴포넌트(Component)]]의 생성, 변경, 소멸 과정을 뜻한다.
- 라이프사이클 [[메서드(Method)]]의 종류는 아홉가지이다. 
- Will 접두사가 붙은 메서드는 어떤 작업을 작동하기 전에 실행되는 메서드이다.
- Did 접두사가 붙은 메서드는 어떤 작업을 작동한 후에 실행되는 메서드이다.
- 라이프사이클 총 세가지, 즉 마운트, 업데이트, 언마운트 카테고리로 나눈다.

![[Pasted image 20240116220818.png]]


![[Pasted image 20240115163453.png]]

## 마운트

- [[DOM(Document Object Model)]]이 생성되고 웹 브라우저상에서 나타나는 것을 마운트(Mount)라고 한다.
- 이때 호출하는 [[메서드(Method)]]는 다음과 같다.

![[Pasted image 20240115163609.png]]



- [[constructor()]]: [[컴포넌트(Component)]]를 새로 만들 때마다 호출되는 [[클래스(Class)]] [[생성자(Constructor)]] 메서드이다.
- [[getDerivedStateFromProps()]]: [[props]]에 있는 값을 [[state]]에 넣을 때 사용하는 메서드이다.
- [[JavaScript/render()]]: 준비한 UI를 렌더링하는 메서드이다.
- [[componentDidMount()]]: [[컴포넌트(Component)]]가 웹 브라우저상에 나타나 후 호출하는 메서드이다.
### 업데이트

- 컴포넌트는 다움과 같은 총 네가지 경우 업데이트([[리렌더링(Re-rendering)]])한다.
	1. props가 바뀔 때
	2. state가 바뀔 때
	3. 부모 컴포넌트가 리렌더링 될 때
	4. this.forceUpdate로 강제 렌더링을 트리거할 때

- 이렇게 컴포넌트를 업데이트할 때는 다음 메서드를 호출한다.

![[Pasted image 20240115164222.png]]

- [[컴포넌트(Component)]]는 다양한 이유로 업데이트될 수 있다.
	1. 부모 컴포넌트에서 넘겨주는 [[props]]가 바뀔 때, 컴포넌트에 전달하는 props의 값이 바뀌면 컴포넌트 렌더링이 이루어진다.
	 2. 컴포넌트 자신이 들고 있는 [[state]]가 [[setState]]([[useState()]])를 통해 업네이트될 때이다.
	 3. 부모 컴포넌트가 리렌더링될 때, 자신에게 할당된 [[props]]가 바뀌지 않아도, 또는 자신이 들고 있는 state가 바뀌지 않아도 부모 컴포넌트가 리렌더링되면 자식 컴포넌트 또한 리렌더링된다.

- ﻿﻿[[getDerivedStateFromProps()]]: 이 메서드는 마운트 과정에서도 호출되며, 업데이트가 시작하기 전에도 호출된다. props의 변화에 따라 state 값에도 변화를 주고 싶을 때 사용한다.

- ﻿﻿[[shouldComponentUpdate()]]
	- 컴포넌트가 리렌더링을 해야 할지 말아야 할지를 결정하는 메서드이다. 
	- 이 메서드에서는 true 혹은 false 값을 반환해야 하며, true를 반환하면 다음 라이프사이클 메서드를 계속 실행하고, false를 반환하면 작업을 중지한다.
	- 즉, 컴포넌트가 리렌더링 되지 않는다. 
	- 만약 특정 함수에서 this.forceUpdate 함수를 호출한다면 이 과정을 생략하고 바로 [[JavaScript/render()]] 함수를 호출한다.

- [[JavaScript/render()]]: 컴포넌트를 리렌더링합니다.

- ﻿﻿[[getSnapshotBeforeUpdate()]]: 컴포년트 변화를 DOM에 반영하기 바로 직전에 호출하는 메서드이다.

- ﻿﻿[[componentDidupdate()]]: 컴포넌트의 업데이트 작업이 끝난 후 호출하는 메서드이다.

## 언마운트

- 마운트의 반대 과정, 즉 컴포넌트를 [[DOM(Document Object Model)]]에서 제거하는 것을 언마운트(unmount)라고 한다.
 
![[Pasted image 20240115170757.png]]

- [[componentWillUnmount()]]: 컴포넌트가 웹 브라우저상에서 사라지기 전에 호출하는 메서드이다.


## 에러 검출

- [[componentDidCatch()]]
