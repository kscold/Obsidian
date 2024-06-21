- 자바스크립트에서 backtick 문자를 사용해서 문자열을 표현한 것을 템플릿 리터럴이라고 한다.

- 템플릿 리터럴을 사용하면 불 바꿈을 쉽게할 수 있고 문자열 내부에 표현식을 포함할 수 있게 된다.


## 줄바꿈 표현

```js
console.log(`string text line 1
			 String text line 2`);

// >> string text line 1
// >> string text line 2
```


## 문자열에서 변수 사용

```js
const a = 5;
const b = 10;

console.log(`Fifteen is ${a + b} and
			not ${2 * a + b}.`)
// >> Fifteen is 15
// >> and not 20.
```
