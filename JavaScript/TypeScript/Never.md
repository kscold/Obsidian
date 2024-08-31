- 절대 발생할 수 없는 타입으로, 함수의 끝에 절대 도달하지 않는다는 의미를 지닌 타입이다.


```ts
function error(message: string): never {
	throw new Error(message)
}

function fail() {
	return error('something failed');
} // 반환 type은 error 로 추론

function infiniteLoop(): never {
	while(true) {
	}
} //never를 반환하는 함수는 함수의 마지막에 도달할 수 없음
```