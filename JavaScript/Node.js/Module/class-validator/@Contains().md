- [[class-validator]]에서 해당 필드([[열(Column)]])가 값이 특정 하위 문자열(substring)을 포함하는지 확인하는 [[데코레이터(Decorator)]]이다.


## 예시

- 아래 코드처럼 작성하면 url 필드에 `'example'`을 가지고 있는지 확인한다.

```ts
@Contains('example')
url: string;    
```