- [[HTML(Hyper Text Markup Language)]]에서도 충분히 폼([[<form>]]) 데이터를 다룰수 있고 자바스크립트([[노드(Node.js)]])에서 [[AJAX(Asynchronous JavaScript and XML)]]로 폼([[<form>]]) 전송을 할 일이 거의 없다고 보면 된다.

- 하지만 [[HTML(Hyper Text Markup Language)]]이 아닌 자바스크립트 단 에서 form 전송 동작이 필요한 경우가 있는데, 이미지 같은 멀티미디어 파일을 페이지 전환 없이 폼 데이터를 [[비동기(asynchronous)]]로 제출 하고 싶을 때나, 자바스크립트로 좀더 타이트하게 폼 데이터를 관리하고 싶을때 formData [[객체(Object)]]를 이용한다고 보면 된다.

## FormData 메서드
### formData.append() 매개변수([[name]], [[value]] , file(Option))

- 기본적으로 [[<input>]] 태그의 [[name]]과 [[value]] [[속성(Property)]]에 해당된다.
- formData.append(name, value) 함수를 이용해 데이터를 넣을시에 value는 문자열로만 입력 된다.   
- 만일 문자열 이외의 데이터 타입을 넣으면 무시되고 문자열로 자동 변환 된다.
- 3번째 [[매개변수(parameter)]]를 대입하면 [[<input>]]의 type이 file이 된다.
### formData.append() 매개변수([[name]])

- key가 존재하는 지 확인한다.
- boolean 값으로 반환한다.


```js
formData.append(name, value)
// form의 name 과 value 를 필드의 추가
// input의 name 속성과 value 입력값 역할을 한다고 생각 하면 됨

formData.append(name, blob, fileName)
// input 의 type 이 'file' 인 경우에 사용
// fileName은 file의 이름의 해당

formData.delete(name)
// 주어진 name으로 필드를 제거

formData.get(name)
// 주어진 name의 해당 하는 필드 value를 반환

formData.getAll(name)
// append함수로 추가시 name이 중복 가능
// 따라서 주어진 name 의 해당 하는 필드의 모든 value를 반환

formData.has(name)
// 주어진 name 의 해당하는 필드가 있을 경우 true, 없으면 false를 반환

formData.set(name, value)
formData.set(name, blob, fileName)
// set 함수는 append 함수 처럼 필드를 추가
// append와 비슷한 set 메서드는 set도 추가를 해주기는 하지만, 기존 key가 있으면 그 key값을 모두 덮어씌움
```

## FormData 메서드 사용 방법

```js
// 자바스크립트로 직접 form 태그를 생성
let formData = new FormData(); // 새로운 폼 객체 생성

formData.append('name','hyemin'); // 폼 데이터를 스크립트로 추가
formData.append('item','hi'); // <input name="item" value="hi">와 같음
formData.append('item','hello'); // <input name="item" value="hello">
```


```js
// 만일 HTML에 미리 form 태그가 있으면 제이쿼리나 자바스크립트로 가져올 수도 있음

// 제이쿼리인 경우
let formData1 = new FormData($("#formId")); 

// 자바스크립트로 가져 올 경우
let formData2 = new FormData(document.getElementById("formId"));
```

- 위처럼 폼 객체에 append() [[메서드(Method)]]드로 key와 value 값을 차례로 추가해주면, 곧 input 태그에 값을 입력하는 것과 같은 효과를 가진다.

- 참고로 값은 무조건 문자열로 자동 변환 된다.
- [[객체(Object)]]나 [[배열(Array)]] 같은 복잡한 데이터는 넣을 수 없다.


```js
let formData = new FormData(); // 새로운 폼 객체 생성
formData.append('item', 'hi');

// key가 존재하는 지 확인. 값이 boolean type으로 반환
formData.has('item'); // true
formData.has('money'); // false

// 값의 첫 번째 값이 반환됩니다.
formData.get('item'); // hi. 

// 중복된 key값을 모두 가져올 때 배열 형식으로 가져옵니다.
formData.append('item', 'hello'); // 똑같은 item 키에 값을 또 추가
formData.getAll('item'); // ['hi','hello']
```


```js
// 중복 된 값을 넣을때는 배열로 한꺼번에 append할 수 있습니다.
formData.append( 'test', ['hi','hyemin'] );
formData.get('test'); // hi,hyemin

// 삭제
formData.delete('test');
formData.get('test'); // null값이 들어갑니다.

// item값을 수정합니다.
formData.set('item','test2');
formData.getAll('item); // ['test2']
```

#### **formData 값 콘솔 출력**

formData 객체를 생성하고 ~~append~~ 를 통해 key와 value 값들을 아주 많이 넣었다고 하자. 그러면 내가 어떠한 폼 데이터들을 넣었는지 확인하고 싶을때, formData 객체를 ~~console.log~~로 출력해 볼 것이다.

하지만 다음 이미지와 같이 폼 데이터의 keys와 values 값들을 어디에도 확인할 수 없다.

[![formData-console-log](https://blog.kakaocdn.net/dn/IeWzo/btrfw5ApM3d/kQQwFE8l11rh2BoY62f4uK/img.png)](https://blog.kakaocdn.net/dn/IeWzo/btrfw5ApM3d/kQQwFE8l11rh2BoY62f4uK/img.png)

왜냐하면 자바스크립트에서 FormData 객체란 단순한 객체가 아니며 XMLHttpRequest 전송을 위하여 설계된 **특수한 객체 형태**이기 때문이다. 그래서 간단하게 문자열 화할 수 없어, ~~console.log~~를 사용하여 확인이 불가능한 것이다.

그렇지만 전혀 확인할 수 있는 방법이 없는 것은 아니다.

#### **formData 값 순회하기**

여러개의 폼 데이터들은 기본적으로 이터러블 하기 때문에 for문 으로 순회 할수도 있다. 그리고 이를 통해 폼 데이터 객체에 어떠한 값들이 들어있는지 확인이 가능하다.

javascript

```
let formData = new FormData();
formData.append('key1', 'value1');
formData.append('key2', 'value2');

// 폼 객체 key 값을 순회.
let keys = formData.keys();
for (const pair of keys) {
    console.log(pair); 
}

// 폼 객체 values 값을 순회.
let values = formData.values();
for (const pair of values) {
    console.log(pair); 
}

// 폼 객체 key 와 value 값을 순회.
let entries = formData.entries();
for (const pair of entries) {
    console.log(pair[0]+ ', ' + pair[1]); 
}
```

#### **formData에 이미지 담기**

이미지와 같은 멀티미디어 파일을 폼 데이터에 담고 싶다면, 아래 코드와 같이 formData 객체를 생성하고, 이미지 파일이 담긴 input 태그를 ~~querySelector~~ 로 받아와서 ~~files[0]~~ 를  append로 더해주면 된다.

			
        

>             
> 
> Tip
> 
>             
> 
> 이미지는 base64, buffer, 2진 data 형식으로 서버로 전송해도 된다. ([참고 글](https://inpa.tistory.com/entry/JS-%F0%9F%93%9A-Base64-Blob-ArrayBuffer-File-%EB%8B%A4%EB%A3%A8%EA%B8%B0-%EC%A0%95%EB%A7%90-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0-%EC%89%BD%EA%B2%8C-%EC%84%A4%EB%AA%85))
> 
>         

html

```
<body>
    <input type="file" id="fileInput">
    <button type="submit" id="sendButton">전송</button>
 
    <script>
        const fileInput = document.querySelector("#fileInput");
        const sendButton = document.querySelector("#sendButton");
 
        sendButton.addEventListener("click",function(){
 
            var formData = new FormData();
            // form Data 객체 생성
            formData.append("attachedImage", fileInput.files[0]);
            // 파일 인풋에 들어간 파일들은 files 라는 리스트로 저장된다.
            // input에 multiple을 선언해 여러개의 파일을 선택한 경우가 아니라면 files[0] 으로 input에 추가한 파일 객체를 찾을 수 있다.
            
        });
 
    </script>
</body>
```

#### **formData 값을 객체로 받기**

이번에는 좀더 심화된 응용 예제이다. 만일 폼 데이터에 넣을 key-value 값들을 객체로 관리하고 싶은 경우 다음과 같이 리팩토링 할 수 있다.

javascript

```
// 폼데이터에 넣을 key-value 값들을 객체로 관리
const obj = {
    first: 'Akash',
    middle: 'Rishi',
    last: 'Mittal',
}
    
const formData = new FormData();

Object.entries(obj).forEach(item => formData.append(item[0], item[1]));
// 성능을 따진다면, 고차 함수 대신 for문을 이용해도 된다.
// for(let key in obj) {
//   formData.append(key, obj[key])
// }

// 폼데이터 값 출력
let entries = formData.entries();
for (const pair of entries) {
    console.log(pair[0]+ ', ' + pair[1]); 
}
```

[![formdata-object](https://blog.kakaocdn.net/dn/cVcnds/btrJWmk6rI1/Ivn3gIkiV2vesbMvUIEKE0/img.png)](https://blog.kakaocdn.net/dn/cVcnds/btrJWmk6rI1/Ivn3gIkiV2vesbMvUIEKE0/img.png)

반대로 formData 객체의 폼 데이터를 자바스크립트 객체로 환원도 가능하다.

javascript

```
const obj2 = {};
formData.forEach((value, key) => obj2[key] = value);

console.log(obj2); // {first: 'Akash', middle: 'Rishi', last: 'Mittal'}
```

---

### **FormData 서버 전송하기**

본래 HTML에서 폼데이터를 서버로 보낼때는 다음과 같이 action 과 method 속성 부분을 정의 해서 input submit으로 보냈었다.

html

```
<form action="/action_page.php" method="get">
  <input type="text" id="fname" name="fname"><br><br>
  <input type="text" id="lname" name="lname"><br><br>
  <input type="submit" value="Submit">
</form>
```

하지만 formData()는 자바스크립트 클래스이기에, 서버에 전송하려면 [fetch apiVisit Website](https://inpa.tistory.com/entry/JS-%F0%9F%93%9A-AJAX-%EC%84%9C%EB%B2%84-%EC%9A%94%EC%B2%AD-%EB%B0%8F-%EC%9D%91%EB%8B%B5-fetch-api-%EB%B0%A9%EC%8B%9D)를 이용해야 한다.

#### **fetch로 폼데이터 전송하기**

여기서 유의해야 할 점은, 자바스크립트에서 서버로 form data를 보내려면 body 속성 부분을 일반적인 json이나 객체 타입 형태가 아닌 form data 타입 형식에 맞춰서 보내야 한다는 점이다.

javascript

```
var formData = new FormData();
formData.append('key1', 'value1');
formData.append('key2', 'value2');

fetch('https://httpbin.org/post', {
    method: 'POST',
    cache: 'no-cache',
    body: formData // body 부분에 폼데이터 변수를 할당
})
.then((response) => response.json())
.then((data) => {
    console.log(data);
});
```

			
        

>             
> 
> Tip
> 
>             
> 
> formData를 보낼때, header 부분은 브라우저가 자동으로 설정해주기 때문에 Content-Type을 application/x-www-form-urlencoded ..등 따로 지정할 필요가 없다.
> 
>         

#### **URLSearchParams**

만일 위처럼 formData 객체에 일일히 ~~append~~ 하여 폼 데이터 값을 구성하는 것이 번거롭다면, ~~URLSearchParams()~~ 을 사용하면 일반 객체형태를 formdata형식으로 자동 변환 해주어 보다 가독성 좋게 전송할 수 있다.

javascript

```
fetch('https://httpbin.org/post', {
    method: 'POST',
    cache: 'no-cache',
    body: new URLSearchParams({ // 일반 객체를 fordata형식으로 변환해주는 클래스
        aaa: 'a1',
        bbb: 'b1'
    })
})
.then((response) => response.json())
.then((data) => {
    console.log(data);
});
```

출처: [https://inpa.tistory.com/entry/JS-📚-FormData-정리-fetch-api](https://inpa.tistory.com/entry/JS-%F0%9F%93%9A-FormData-%EC%A0%95%EB%A6%AC-fetch-api) [Inpa Dev 👨‍💻:티스토리]