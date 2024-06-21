- [[비동기(asynchronous)]] 처리를 위해 사용되는 [[async await]]는 한 세트이기 때문에, await 혼자서는 동작이 불가능했지만 ES2022부터는 모듈의 최상위 레벨에서 await를 사용할 수 있게 되었다.

```js
// todoList.mjs
let todoList;

(async () => {
	const response = await fetch("https ://jsonplaceholder.typicode.com/todos/1");
	todoList = await response.json();
})();

export { todoList };

// index.mjs
import { todoList } from "./todoList.mjs";

console.log(todoList); // undefined
```

- 위의 코드처럼 todoList.mjs([[ES module]])는 IIAFE를 활용해 todoList를 조회한 후 결과를 내보내는 모듈이며, index.mjs은 todoList.mjs의 결과를 [[import]]하여 출력하는 [[모듈(Module)]]이다.

- 하지만 index.mjs에서 todoList.mjs의 [[비동기(asynchronous)]] 처리가 완료되지 않은 시점에 todoList로 접근이 가능하기 때문에 [[undefined]]를 출력하는 것을 확인할 수 있다.
- 이를 해결하기 위한 첫 번째 방법으로 todoList.mjs의 [[비동기(asynchronous)]] 처리가 완료될 때까지 일정 시간을 기다린 후 todoList로 접근하는 방법이 있다.

```js
setTimeout(() => {
	console.log(todoList); 
	// {userId: 1, id: 1, title: 'delectus aut autem', completed: false}
}, 100);
```

- 결과는 잘 출력되고 있지만 아시다시피 이 방법은 todoList.mjs의 [[비동기(asynchronous)]] 처리가 완료됐다는 것을 보장해 주지 않는다. 
- 비동기 처리가 예상했던 100ms보다 늦게 끝날 수도 있기 때문이다.

- 이를 해결하기 위한 다른 방법으로는 todoList.mjs에서 [[Promise]] [[객체(Object)]]를 반환하는 방법이 있다.

```js
// todoList.mjs
let todoList;

export default (async () => {  
	const response = await fetch("https://jsonplaceholder.typicode.com/todos/1");  
	todoList = await response.json();
})();

export { todoList };

// index.mjs
import promise, { todoList } from "./todoList.mjs";

promise.then(() => {  
	console.log(todoList); 
	// {userId: 1, id: 1, title: 'delectus aut autem', completed: false}
});
```

- 이제 index.mjs에서는 [[Promise]] [[객체(Object)]]의 [[then()]] [[메서드(Method)]]를 사용하여 [[비동기(asynchronous)]] 처리 직후 todoList에 접근 가능하다.

- 하지만 이 코드는 여전히 복잡할 뿐만 아니라, promise를 사용하지 않아도 todoList로 접근이 가능하기 때문에 위험요소를 포함하고 있다.

- 따라서 밑의 코드는 Top-level await를 활용하여 해당 문제들을 해결한다.

```js
// todoList.mjs
let todoList;

const response = await fetch("https://jsonplaceholder.typicode.com/todos/1");
todoList = await response.json();

export { todoList };

// index.mjs
import { todoList } from "./todoList.mjs";

console.log(todoList); 
// {userId: 1, id: 1, title: 'delectus aut autem', completed: false}
```

- Top-level await를 사용한 todoList.mjs은 [[import]] 하는 index.mjs는, todoList.mjs의 await가 모두 실행되기 전(비동기 처리가 완료되기 전까지) 동작을 중지하게 된다.

- 마치 Top-level await를 사용한 [[모듈(Module)]]이 하나의 거대한 async 함수처럼 동작하게 된다.
- 따라서 B 모듈에서는 todoList로 바로 접근을 해도, 비동기 처리가 완료됐다는 것을 보장하기 때문에 원하던 결과를 얻을 수 있게 된다.
