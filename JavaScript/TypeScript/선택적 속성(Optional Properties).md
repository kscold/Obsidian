- [[속성(Property)]] 선언 시 이름 끝에 ?를 붙여서 표시한다.
- 인터페이스([[interface]])에 속하지 않는 [[속성(Property)]]의 사용을 방지하면서, 사용 가능한 프로퍼티를 기술할 때 사용한다.

- [[객체(Object)]] 안의 몇 개의 [[속성(Property)]]만 채워 함수에 전달하는 "option bags"같은 패턴에 유용하다.
- Option 사항은 없어도 되지만, 만약 존재한다면, 반드시 정의해둔 Type과 일치해야 한다.


## 예시

```ts
interface CompanyInfo {
	name: string;
	chairman?: string;
}

let obj: CompanyInfo = {
	name: 'FaceBook'
};
```