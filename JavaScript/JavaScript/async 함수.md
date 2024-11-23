- async 함수의 성질은 일반 [[함수(Function)]]처럼 사용가능하다.

- [[Promise]] [[객체(Object)]]로도 사용할 수 있다. 
- 이때문에 [[Promise.all()]]에도 적용해서 사용할 수 있다.


## 문법

- await의 경우, 항상 async라는 이름의 함수 수정자가 있는 [[함수(Function)]] 몸통에서만 사용할 수 있다.

```js
const hiasync = async () => {
	await Promise 객체 혹은 값
}
```


## async 함수의 특징

- async 함수는 값을 반환할 수 있다. 

- 이때 반환값은 [[Promise]] 형태로 변환되므로 [[then()]] [[메서드(Method)]]를 통해서 반환되는 값을 얻어야 한다. 

```js
const hi = async ()=>{
    return [1, 2, 3];
}

const asyncReturn = async ()=>{
    const result = await hi()
    console.log('value0:', result)
    return result;
}

asyncReturn().then(value => 
    console.log('value1: ', value))
```

- 제일 외부에서는 [[then()]]을 통해서 값에 접근해야한다.
