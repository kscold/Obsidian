- [[JWT(JSON Web Token)]]에서 [[payload]]는 토큰에 담기는 실제 데이터를 의미한다.
- [[payload]]는 전송되는 데이터를 의미한다.
- [[payload]]에는 사용자 정보나 권한 등 필요한 데이터를 담을 수 있다.


## payload의 구조

```ts
const payload = {
    username: 'tae',
    age: 25,
    address: 'seoul'
}
```

- [[payload]]에는 민감한 정보를 담지 않는 것이 좋다.

- [[JWT(JSON Web Token)]]의 [[payload]]는 암호화되지 않고 인코딩만 되어있어 누구나 디코딩하여 볼 수 있기 때문이다.