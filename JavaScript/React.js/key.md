- 리액트에서 key는 [[컴포넌트(Component)]] [[배열(Array)]]을 렌더링했을 때 어떤 원소에 변동이 있었는지 알아내려고 사용한다.
- 예를 들어 유동적인 데이터를 다룰 때는 원소를 새로 생성할 수도, 제거할 수도 있다.
- key가 없을 때는 [[Virtual DOM]]을 비교하는 과정에서 리스트를 순차적으로 비교하면서 변화를 감지한다.
- key가 있다면 이 값을 사용하여 어떤 변화가 일어났는지 더욱 빠르게 알 수 있다.

## key 설정

- key 값을 설정할 때는 map함수의 인자로 전달되는 함수 내부에서 [[컴포넌트(Component)]] [[props]]를 선정하듯이 설정하면 된다.
- key 값은 언제나 유일(unique)해야 한다.

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