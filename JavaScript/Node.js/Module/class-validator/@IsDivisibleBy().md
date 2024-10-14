- @IsDivisibleBy() [[데코레이터(Decorator)]]는 값이 특정 숫자로 나누어떨어지는지(즉, 나머지가 0인지) 검증하는 데 사용된다.

- 주로 숫자 필드([[열(Column)]])에서 특정 숫자의 배수인지 확인할 때 유용하다.

- 나눗셈 결과가 정수여야 하므로, 소수점 숫자도 검증에 실패하게 된다.


## 예시

```ts
class ProductDto {   
	@IsDivisibleBy(5)   
	quantity: number; 
}
```

- @IsDivisibleBy(5)는 quantity 필드 값이 5로 나누어떨어지는지 확인한다.
- 예를 들어, `quantity`가 10, 15, 20이면 유효하지만, 12 또는 7일 경우 검증에서 실패한다.