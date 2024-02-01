- 문자열 변수나 값을 을 정수 [[변수(Variable)]]나 값으로 만들 때 사용한다.
- 이 parseInt() 함수는 문자열을 파싱해서 숫자나 NaN(Not a Number)을 리턴한다.
- 파이썬의 int() 형변환과 같다고 생각하면 된다.

## 문법

```js
parseInt(string, radix);
```

- string 숫자로 변환할 문자열이다.
- radix(optional) string 문자열을 읽을 진법(수의 진법 체계의 진법)을 나타낸다.(2~36의 수)

- string을 정수로 변환한 값을 리턴한다.
- 만약, string의 첫 글자를 정수로 변경할 수 없으면 NaN(Not a Number)값을 리턴합니다.

## 예시

```js
parseInt("10"); // 10
parseInt("-10"); // -10
parseInt("10.9"); // 10
parseInt(10); // 10
parseInt("10n"); // 10
parseInt("10nnn13"); // 10
parseInt("    10"); // 10
parseInt("10      "); // 10
parseInt("k10"); // NaN
parseInt(""); // NaN
```