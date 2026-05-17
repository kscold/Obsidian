- [[RxJS(Reactive Extensions For JavaScript)]]에 속한 [[메서드(Method)]]로 부작용없이 로깅과 같은 액션을 수행한다.

- pipeable 연산자를 사용중이라면, `do` 대신 `tap` 을 사용하는 것이 좋다.


## 문법

```ts
tap(nextOrObserver: function, error: function, complete: function): Observable
```


## 예시

- 아래 코드는 tap을 사용하여 로그찍는 예시이다.

```ts
// RxJS v6+
import { of } from 'rxjs';
import { tap, map } from 'rxjs/operators';

const source = of(1, 2, 3, 4, 5);

// 'tap'을 사용하여 소스로부터 로그 값을 찍음
const example = source.pipe(
	tap(val => console.log(`BEFORE MAP: ${val}`)),
	map(val => val + 10),
	tap(val => console.log(`AFTER MAP: ${val}`))
);

// 'tap'은 값을 바꾸지 않음
// 결과: 11...12...13...14...15
const subscribe = example.subscribe(val => console.log(val));
```

-  [[객체(Object)]]에서 tap 사용할 수 있다.

```ts
// RxJS v6+
import { of } from 'rxjs';
import { tap, map } from 'rxjs/operators';

const source = of(1, 2, 3, 4, 5);

// tap은 next, error, complete을 사용할 수 있음
const example = source
	.pipe(
	    map(val => val + 10),
		tap({
		    next: val => {
		    console.log('on next', val); // on next 11
	      },
	    error: error => {
	        console.log('on error', error.message);
	    },
	    complete: () => console.log('on complete')
	})
).subscribe(val => console.log(val));

// 결과: 11, 12, 13, 14, 15
```