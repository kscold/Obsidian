- @Transform() [[데코레이터(Decorator)]]는 [[class-transformer]] [[모듈(Module)]]에서 제공하는 [[데코레이터(Decorator)]]로, [[클래스(class)]]의 특정 [[속성(Property)]]을 [[직렬화(Serialization)]] 또는 [[역직렬화 (Deserialization)]]할 때 원하는 방식으로 값을 변환할 수 있도록 도와준다.

- 즉, 데이터를 변환하는 로직을 사용자 정의할 수 있으며, [[JSON(Java Script Object Notation)]]이나 평범한 [[객체(Object)]]에서 [[클래스(class)]] [[인스턴스(Instance)]]로 변환하거나, 반대로 [[클래스(class)]]를 [[JSON(Java Script Object Notation)]]으로 변환할 때 이 [[데코레이터(Decorator)]]를 사용하여 데이터를 가공할 수 있다.

- 예를 들어 날짜 형식을 문자열로 변환하거나 특정 형식으로 출력할 때니, 문자열을 숫자, 또는 그 반대로 변환할 때 사용한다.
- 즉, 값에 특정 로직을 적용해서 변환할 때 사용된다.


## 문법

- @Transform()은 transform이라는 [[콜백 함수(Callback Function)]]를 받아, 이 함수 안에서 값 변환 로직을 정의할 수 있다.

```ts
class 클래스명 { @Transform(({ value }) => 특정 로직, { toPlainOnly: true }) 
	firstName: string;
}	
```
### toClassOnly 

- [[직렬화(Serialization)]] 시 변환을 적용하지 않고 [[역직렬화 (Deserialization)]]([[클래스(class)]] [[인스턴스(Instance)]]로 변환) 시에만 변환 로직을 적용할 때 사용한다.
### toPlainOnly

- [[역직렬화 (Deserialization)]] 시에는 변환을 적용하지 않고 [[직렬화(Serialization)]]([[클래스(class)]]를 평범한 [[객체(Object)]]로 변환) 시에만 변환 로직을 적용할 때 사용한다.


## 예시

```ts
import { Transform, plainToClass } from 'class-transformer';

class User {
    @Transform(({ value }) => value.toUpperCase()) // 값을 대문자로 변환
    firstName: string;
	
    @Transform(({ value }) => value.toUpperCase()) // 값을 대문자로 변환
    lastName: string;
	
    @Transform(({ value }) => new Date(value), { toClassOnly: true }) // 날짜 문자열을 Date 객체로 변환
    birthDate: Date;
	
    constructor(firstName: string, lastName: string, birthDate: Date) {
        this.firstName = firstName;
        this.lastName = lastName;
        this.birthDate = birthDate;
    }
}

const plainUser = {
    firstName: 'john',
    lastName: 'doe',
    birthDate: '1990-01-01'
};

// 평범한 객체에서 클래스 인스턴스로 변환
const userInstance = plainToClass(User, plainUser);
console.log(userInstance);

/*
User {
  firstName: 'JOHN',
  lastName: 'DOE',
  birthDate: 1990-01-01T00:00:00.000Z
}
*/
```