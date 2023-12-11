
input 태그의 readonly 속성

```html
<input type="text" value="only read" readonly>
```


결과화면 : 읽기만 가능할 뿐 값 변경은 불가능.

![](https://t1.daumcdn.net/cfile/tistory/991774335A0005752A)

  

input 태그의 disabled 속성
```html
<input type="text" value="only read" disabled>
```

결과화면 : 읽기만 가능할 뿐 값 변경은 불가능

![](https://t1.daumcdn.net/cfile/tistory/994457335A00077507)


- readonly 와 disabled 의 차이 

**form** 으로 값을 보낼때 **disabled**의 값은 전송되지 않는다.