- [[타입스크립트(TypeScript)]]에서 옵셔널은 기본적으로 값이 있을 수도 없을 수도 있는 [[변수(Variable)]]를 만드는 것이다.
- 즉, 옵셔널(Optional)은 있을 수도, 없을 수도 있는 [[변수(Variable)]]를 저장하기 위해 사용한다.


## 문법

- [[타입스크립트(TypeScript)]]에서 값이 없는 [[변수(Variable)]]는 [[undefined]]를 반환한다.

```typescript
const kimCheolSu : Student = {
  name: "김철수",
  grade: 2
}

typeof(kimCheolSu.phoneNumber) // Undefined
```

- 따라서, 옵셔널을 사용하여 if문을 사용할 경우 추가적인 선언이 필요하다.

```ts
if (kimeCheolSu.phonNumber == 01012345678) { 
	// Do Somthing
} 
// Error 발생 - Object is possibly 'undefined'.


if (kimCheolSu.phonNumber && kimeCheolSu.phonNumber == 01012345678) {
	// Do Something
}  
```


## 옵셔널의 사용 예시

- 예를 한번 들어보면, 한 반의 학생들의 정보가 이렇게 있다고 가정한다.

| 이름  | 학년  | 전화번호        |
| --- | --- | ----------- |
| 홍길동 | 1   | 01012345678 |
| 김철수 | 2   |             |
| 김영희 | 2   | 01098765432 |
| 박민수 | 3   | 01011112222 |

  - 이때, 김철수는 전화번호가 없으므로, 이런식으로 type을 설정할 경우 오류가 생긴다.

```ts
type Student = {
	name: string,
	grade: number,
	phoneNumber: number
}
```

- 따라서 [[변수(Variable)]] 이름 옆에 ?를 함으로써 Optional을 사용할 수 있다.  

```ts
type Student = {
	name: string,
	grade: number.
	phoneNumber?: number
}
```


