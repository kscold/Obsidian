- localStorage를 사용하면, 브라우저에 key-value 값을 Storage에 저장할 수 있다.

- 저장한 데이터는 세션간에 공유된다.
- 즉, 세션이 바뀌어도 저장한 데이터가 유지된다.
- 따라서 브라우저가 종료되면 사라지는 [[sessionStorage]]와 큰 차이를 가진다.

## localStorage 메서드

### setItem() 

- key, value 형식으로추가한다.

- localStorage에 아이템을 추가하기 위해서는 setItem() 함수를 사용한다.
- 이때 [[객체(Object)]]나 [[배열(Array)]]을 저장할 경우, `[object Object]` 형식으로 표현되는데 이 경우를 방지하기 위해서는 [[JSON.stringify()]]를 사용하여 문자열로 바꾸어서 저장하면 된다.

```js
window.localStorage.setItem(key, value)
```
### getItem()

- localStorage의 아이템(value)을 읽기 위해서는 getItem() 함수를 사용한다.

```js
window.localStorage.getItem(key)
```

```js
// setItem
window.localStorage.setItem('name', 'anna');
window.localStorage.setItem('age', '20'); 

// getItem
const name = window.localStorage.getItem('name');
const age = window.localStorage.getItem('age'); // 결과 출력
document.write(name); // 
annadocument.write('<br/>');
document.write(age);  // 20
```

- setItem() 함수를 사용하여 localStorage에 key-value 를 저장한다.
- localStorage에는 문자열만을 저장할 수 있다.
- 따라서, 20을 숫자로 저장하더라도, 문자열로 저장된다.
- getItem() 함수에 key를 전달하여 localStorage에 저장된 값을 읽어 왔다.

### removeItem()

- localStorage의 요소(item)를 삭제한다.

```js
window.localStorage.removeItem(key);
```

```js
// setItem
window.localStorage.setItem('name', 'anna');
window.localStorage.setItem('age', '20'); // removeItem
window.localStorage.removeItem('name');
window.localStorage.removeItem('grade'); // getItem

const name = window.localStorage.getItem('name');
const age = window.localStorage.getItem('age'); // 결과 출력

document.write(name); // null
document.write('<br/>');document.write(age); // 20
```

- removeItem() 함수를 사용하여 key가 'name'인 아이템을 삭제하였다.

- 'grade'라는 key는 localStorage에 존재하지 않는데, removeItem에 존재하지 않는 key를 파라미터로 전달하면, 아무일도 일어나지 않는다.

### clear()

- 도메인 내의 localStorage 값을 삭제한다.

```js
window.localStorage.clear();
```

```js
// setItem
window.localStorage.setItem('name', 'anna');
window.localStorage.setItem('age', '20'); // clear
window.localStorage.clear(); // getItem

const name = window.localStorage.getItem('name');
const age = window.localStorage.getItem('age'); // 결과 출력

document.write(name); // null
document.write('<br/>');
document.write(age); // null
```

- localStorage의 도메인의 모든 아이템을 삭제하였다.

### length

- *localStorage의 전체 요소(item) 갯수이다.

```js
window.localStorage.length;
```

```js
// 초기화
window.localStorage.clear(); // setItem
window.localStorage.setItem('name', 'anna');
window.localStorage.setItem('age', 20); // localStorage item 갯수

const length = window.localStorage.length // 결과 출력

document.write(length); // 2
```

- localStorage에 저장된 값의 갯수를 확인하기 위해서 localStorage의 length 속성을 이용하였다.

### key()

- localStorage의 index로 key값 찾는다.

```js
window.localStorage.key(index);
```

```js
// setItem
window.localStorage.setItem('name', 'anna');
window.localStorage.setItem('age', '20'); // key

const key_1 = window.localStorage.key(0);
const key_2 = window.localStorage.key(1); // 결과 출력

document.write(key_1); // age
document.write('<br/>');
document.write(key_2); // name
```

- key() 함수의 파라미터로 index를 전달하여 해당 index의 key 값을 조회하였다.


## localStorage에 객체, 배열 저장하기

```js
// localStorage에 저장할 객체
const obj = {  
	name : 'anna',  
	age : 20
} 

// localStorage에 저장할 배열
const arr = [1, 2, 3]; 

// 객체, 배열을 JSON 문자열로 변환
const objString = JSON.stringify(obj);
const arrString = JSON.stringify(arr);

// setItem
window.localStorage.setItem('person', objString);
window.localStorage.setItem('nums', arrString); 

// getItem
const personString = window.localStorage.getItem('person');
const numsString = window.localStorage.getItem('nums'); 

// JSON 문자열을 객체, 배열로 변환
const personObj = JSON.parse(personString);
const numsArr = JSON.parse(numsString); 

// 결과 출력
document.write(personString); // {"name":"anna","age":20}
document.write('<br/>');
document.write(personObj); // [object Object]
document.write('<br/>'); 

document.write(numsString); // [1,2,3]
document.write('<br/>');
document.write(numsArr); // 1,2,3
document.write('<br/>');
```

- localStorage 에는 문자열만 저장된다.
- 따라서, localStorage에 객체나 배열를 저장하기 위해서는 [[객체(Object)]]를 문자열로 변환해서 저장해야 한다.

- 여기서는 [[JSON.stringify()]] 함수를 사용하여 객체와 배열을 JSON 문자열로 변환했다.
- JSON 문자열을 localStorage에 저장했다.


## 전체 key, value 가져오기

### [[for]] 문

```js
// 초기화
window.localStorage.clear(); 

// setItem
window.localStorage.setItem('name', 'anna');
window.localStorage.setItem('age', 20); // 모든 key, value 출력

// output =>
//    age : 20
//    name : anna

for(let i = 0; i < window.localStorage.length; i++) {  // key 찾기 

const key = window.localStorage.key(i);    // value 찾기  
const value = window.localStorage.getItem(key);    // 결과 출력 

document.write(key + " : " + value + "<br />");}
```

- length 속성을 사용하여 전체 아이템의 길이를 구하여 for문을 돌고 const key = window.localStorage.key(i); for문의 index 값과 key() 함수를 사용하여 key 이름을 읽었다.

-  이후 const value = window.localStorage.getItem(key); 앞에서 찾아온 key값과 getItem() 함수를 이용하여 해당 key의 value를 읽었다.

### [[for]] ... in 문

```js
// 초기화
window.localStorage.clear(); 

// setItem
window.localStorage.setItem('name', 'anna');
window.localStorage.setItem('age', 20); // 모든 key, value 출력
// output => 
//     age : 20
//    name : anna
//    length : null
//    clear : null
//    getItem : null
//    key : null
//    removeItem : null
//    setItem : null

for(const key in window.localStorage) {  // value 찾기  
	
	const value = window.localStorage.getItem(key);    // 결과 출력
	document.write(key + " : " + value + "<br />");
}
```

- for...in 문을 사용하여 localStorage의 key 목록을 조회하였다.

- 그런데 for...in 문을 사용하면,  사용자가 정의한 key 이외에 localStorage의 built-in 항목까지 조회된다는 단점이 있다.

- 따라서 for...in 문을 사용하고 싶다면 아래 예제와 같이 코드가 수정되어야 한다.

- 아래 코드는 built-in propery 제거된 코드이다.

```js
// 초기화
window.localStorage.clear(); 

// setItem
window.localStorage.setItem('name', 'anna');
window.localStorage.setItem('age', 20); // 모든 key, value 출력

// output => 
//    age : 20
//    name : anna

for(const key in window.localStorage) {
	
	if(window.localStorage.hasOwnProperty(key)) { // value 찾기
		const value = window.localStorage.getItem(key); // 결과 출력 
		document.write(key + " : " + value + "<br />");  
	}
}
```

- for...in 문에서 조회되는 built-in 항목을 제거하기 위해서 hasOwnProperty() 함수를 사용했다.

### Object.keys()  

```js
// 초기화
window.localStorage.clear(); 

// setItem
window.localStorage.setItem('name', 'anna');
window.localStorage.setItem('age', 20); // key 목록 조회 / 출력
const keys = Object.keys(window.localStorage);
document.write(keys);document.write('<br/>'); // 모든 key, value 출력

// output => 
//     age,name
//     age : 20
//     name : anna

for(const key of keys) {    // value 찾기    
	const value = window.localStorage.getItem(key); // 결과 출력    
	document.write(key + " : " + value + "<br />");
}
```

- localStorage의 key 목록을 조회하기 위해 Object.keys() 함수를 사용한다.
