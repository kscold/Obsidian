```json
{
	title: '제목',
	body: "내용",
	tags: ['태그1','태그2']
}
```
위의 형식의 [[스키마(schema)]]와  적용된 [[모델(model)]]을 가지고 있을 때


```js
const schema = Joi.object().keys({// 객체가 다음 필드를 가지고 있음을 검증 객체의 key를 검사
	title: Joi.string().required(), // required()가 있으면 필수 항목
	body: Joi.string().required(), // required()가 있으면 필수 항목
	tags: Joi.array().items(Joi.string()).required(), // 문자열로 이루어진 배열
});

// 검증하고 나서 검증 실패한 경우 에러 처리

const result = schema.validate(ctx.request.body);

if (result.error) {
	ctx.status = 400; // Bad Request
	ctx.body = result.error;
	return;
}

```

위와 같이 Joi 라이브러리를 이용하려 검증


`alphanum()`은 `joi`에서 사용되는 스키마 유형 중 하나다.
이것은 입력 값이 영숫자 (알파벳 문자 또는 숫자)만 포함하도록 유효성을 검사하는 데 사용한다.