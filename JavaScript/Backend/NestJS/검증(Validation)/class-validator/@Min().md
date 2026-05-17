- [[class-validator]]에서 해당 필드([[열(Column)]])가 값이 특정 최소값 이상 값인지 검증하는 [[데코레이터(Decorator)]]이다.


## 예시


```ts
@Min(5) count: number;
```

- `count`는 최소 5 이상이어야 합니다. 5 미만의 값은 유효하지 않다.
