- [[FindOptions]] 인터페이스([[Interface]])의 [[속성(Property)]]으로 FindOptionsCache [[속성(Property)]]을 통해 특정 데이터([[행(Row)]])을 캐싱할 수 있다.
- 즉, 정해진 기간 동안은 여러번 [[쿼리(Query)]]문이 입력되어도 캐싱되어 있기 떄문에 한번만 처리된다.


## 예시

```ts
const users = await userRepository.find({
	cache: true,
})
```

- 아래는 캐싱 기간 직접 정의하여 사용하는 방법이다.

```ts
const users = await userRepository.find({
	cache: 60000,
})
```