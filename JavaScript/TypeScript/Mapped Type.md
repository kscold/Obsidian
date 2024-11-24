- [[타입스크립트(TypeScript)]]의 고급 타입인 맵드 타입(mapped type)이란 기존에 정의되어 있는 타입을 새로운 타입으로 변환해 주는 문법을 의미한다. 


## Mapped Type의 사용 예시

- 예를 들어 인터페이스([[Interface]])에 있는 모든 속성을 루프문 같이 순회해서 [[옵셔널(Optional)]](`?`) 로 바꾸거나 [[readonly]] 로 지정할수 있으며, 아예 지정된 타입을 바꿔서 변경된 타입을 반환할 수도 있다.

[![typescript-mapped-types](https://blog.kakaocdn.net/dn/dS7jVe/btrH3RGxRB2/LjO6CXOXlNKWdhxFKrpdsK/img.png)


## Mapped Type 정리
### [[Partial]]

- [[클래스(class)]]와 [[속성(Property)]] 정의를 모두 [[옵셔널(Optional)]]로 만든다.
### [[Pick]]

- 특정 [[속성(Property)]]만 골라 사용할 수 있다.
- [[Omit]]의 반대이다.
### [[Omit]]

- 특정 [[속성(Property)]]만 생략할 수 있다.
- [[Pick]]의 반대이다.
### [[Intersection]]

- 두 타입의 [[속성(Property)]]을 모두 모아서 사용할 수 있다.
### [[Composition]]

- Mapped Type를 다양하게 조합해서 중첩 적용 가능하다.