- transform() [[메서드(Method)]]는 두개 의 [[매개변수(parameter)]]를 가진다.


- 첫번째 [[매개변수(parameter)]]는 처리가 안된 인자의 값(value)이며 두 번째 [[매개변수(parameter)]]는 인자에 대한 메타 데이터를 포함한 [[객체(Object)]]이다.

- transform() [[메서드(Method)]]에서 return 된 값은 Route 핸들러로 전해진다.
- 만약 예외(Exception)이 발생하면 클라이언트에 바로 전해진다.


## 예시

```ts
export class BoardStatusValidationPipe implements PipleTransform {
	transform(value: any, metadata: ArgumentMetadata) {
		console.log('value', value);
		console.log('metadata', metadata);
		
		return value;
	}
}
```