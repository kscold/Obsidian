- Number [[객체(Object)]]는 숫자를 표현하는 wrapper 객체이다.
- Number 객체는 Java나 C#에서의 double과 비슷하다.
- 즉, Number 객체는 소수점 이하 17자리를 표현할 수 있다.
- Number()는 Number 객체의 [[생성자(Constructor)]]로, Number 객체를 생성한다.

## Number()와 [[parseInt()]]의 차이

- Number()는 오로지 숫자 타입의 문자열만 Number 타입으로 변환한다.

```js
Number('1') // 1  
Number('1st') // NaN  
Number('No1') // NaN  
Number('No1A') // NaN  
  
parseInt('1') // 1  
parseInt('1st') // 1  
parseInt('No1') // NaN  
parseInt('No1A') // NaN
```

- Number()는 소수점을 표시할 수 있다.

```js
Number('1.9') // 1.9  
parseInt('1.9') // 1
```

- parseInt()는 2번째 매개변수를 이용하여, 문자열을 2진법, 16진법 숫자를 10진법으로 변환할 수 있다.

```js
Number('11') // 11  
  
parseInt('11') // 11  
parseInt('11', 2) // 3  
parseInt('11', 16) // 17
```

- null, boolean, empty string 등의 처리에 차이가 있다.

```js
Number() // 0  
Number(true) // 1  
Number(null) // 0  
Number('') // 0  
  
parseInt() // NaN  
parseInt(true) // NaN  
parseInt(null) // NaN  
parseInt('') // NaN
```
