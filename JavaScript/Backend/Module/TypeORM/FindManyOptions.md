


## 문법

```ts
export interface FindManyOptions<Entity = any> extends FindOneOptions<Entity>{
	skip?: number
	
	take?: number
}
```
### skip

- 정렬 후 스킵할 데이터의 갯수를 정할 수 있다.
### take

- 처음 몇개의 데이터를 불러올지 정할 수 있다.


## 예시

```ts
const users = await userRepository.find({
	skip: 10,
	take: 5,
})
```