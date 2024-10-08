- [[리액트(React)]]에서 요소의 [[배열(Array)]](리스트)를 나열할 때는 key를 넣어주어야 한다.
- key는 [[리액트(React)]]가 변경, 추가 또는 제거된 항목을 식별하는데 도움이 된다.

- 요소에 안정적인 ID를 부여하려면 [[배열(Array)]] 내부 요소에 key를 제공해야 한다.
- 주로 [[map()]] [[메서드(Method)]]에서 많이 쓰인다.


### [[CRP(Critical Rendering Path)]]의 재조정(reconciliation)을 위해 사용

- [[Virtual DOM]]을 이용해서 바뀐 부분만 실제 [[DOM(Document Object Model)]]에 적용하기 때문에 key를 이용해서 어떤한 부분이 바뀌었는지 인식할 수 있다.

![[Pasted image 20240626175122.png]]

- 예를 들어 아래 코드에서 3번을 1, 2번 뒤에 추가할 때는 [[리액트(React)]]가 3번만 추가하면 된다고 안다.

```jsx
// 이전
<ul>
	<li>1</li>
	<li>2</li>
</ul>

// 이후
<ul>
	<li>1</li>
	<li>2</li>
	<li>3</li>
</ul>
```

- 아래 코드와 같이 3번을 1, 2번 앞에 추가할 때는 [[리액트(React)]]가 모든 요소가 새롭게 된거라 인식하고 모든 자식 엘리먼트를 새롭게 그려버린다.
- 이 경우 key를 추가해서 1, 2번을 새롭게 그리는 것이 아닌 3번을 추가 후 1, 2번은 자리만 이동해준다.

```jsx
// 이전
<ul>
	<li>1</li>
	<li>2</li>
</ul>

// 이후
<ul>
	<li>3</li>
	<li>1</li>
	<li>2</li>
</ul>
```

- 위와 같은 경우에 [[map()]]의 두 번째 [[매개변수(parameter)]]인 index를 key로 사용하면 key의 대한 value가 변하게 된다.
- 따라서 index를 key로 사용하는 경우는 권장되지 않는다.


## 예시

- [[key]]값을 설정할 때는 [[map()]]의 인자로 전달되는 [[함수(Function)]] 내부에서 [[컴포넌트(Component)]] [[props]]를 선정하듯이 설정하면 된다.
- [[key]]값은 언제나 유일(unique)해야 한다.

```jsx
const articleList = articles.map(article => ()
	<Article
		title={article.title}
		key={article.writer}
		key={article.id}
	/>
));
```

- 하지만 위의 예시처럼 교유 번호(id)가 없을 때에는 map 함수에 전달되는 콜백 함수의 인수인 index 값을 사용하면 된다.

```jsx
const IterationSample = () => {
	const names = ['눈사람', '얼음', '눈', '바람'];
	const nameList = names.map((name, index) => <li key={index}>{name}</li>);
	return <ul>{nameList}</ul>;
}
```