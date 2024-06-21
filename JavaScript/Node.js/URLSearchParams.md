- [[url]]과 마찬가지로 URLSearchParams도 모두 [[노드(Node.js)]]의 내장 [[객체(Object)]]이다.


## URLSearchParams 메서드 

- URL [[생성자(Constructor)]]를 통해 myURL이라는 주소 객체를 만들었다.
- myURL 안에는 searchParms [[객체(Object)]]([[useSearchParams()]]의 일반 객체와 같다.)가 있다.
- 이 객체는 search 부분을 조작하는 다양한 [[메서드(Method)]]를 지원한다.

- [[FormData]]와 활용이 비슷하다.

- myURL.searchParams 대신 new URLSearchParams(myURL.search)로도 같은 결과값을 얻을 수 있다.

### getAll() [[매개변수(parameter)]](key)

- key의 해당하는 모든 값을 가져온다.
- category 키에는 nodejs와 javascript라는 두가지 값이 들어있다.
### get() [[매개변수(parameter)]](key)

- key에 해당하는 첫번째 값만 가져온다.
### has() [[매개변수(parameter)]](key)

- 해당 key가 있는지 없는지를 검사한다.
### keys()

- searchParams의 모든 key를 반복기(iterator) [[객체(Object)]]로 가져온다.
### values()

- searchParams의 모든 값을 반복기(iterator) [[객체(Object)]]로 가져온다.
### append() [[매개변수(parameter)]](key, value)

- 해당 key를 추가한다.
- 같은 키의 값이 있다면 유지하고 하나 더 추가한다.
### set()  [[매개변수(parameter)]](key, value)

- append()와 비슷하지만 모든 key의 value들을 모두 지우고 새로 추가한다.
### delete() [[매개변수(parameter)]] (key)

- 해당 key를 제거한다.
### toString()

- 조작한 searcjParams [[객체(Object)]]를 다시 문자열로 만든다.
- 이 문자열을 search에 대입하면 주소 객체에 반영된다.

```js
const myURL = new URL(
	'http://www.gilbut.co.kr/?page=3&limit=10&category=nodejs&category=javascript'
);

console.log('searchParams:', myURL.searchParams);
console.log('searchParams.getAll():', myURL.searchParams.getAll('category'));
console.log('searchParams.get():', myURL.searchParams.get('limit'));
console.log('searchParams.has():', myURL.searchParams.has('page'));

console.log('searchParams.keys():', myURL.searchParams.keys());
console.log('searchParams.values():', myURL.searchParams.values());


myURL.searchParams.append('filter', 'es3');
myURL.searchParams.append('filter', 'es5');
console.log(myURL.searchParams.getAll('filter'));

myURL.searchParams.set('filter', 'es6');
console.log(myURL.searchParams.getAll('filter'));

myURL.searchParams.delete('filter');
console.log(myURL.searchParams.getAll('filter'));
  
console.log('searchParams.toString():', myURL.searchParams.toString());
myURL.search = myURL.searchParams.toString();
```

- 밑에는 실행결과이다.

```bash
> node searchParams.js                                                                
searchParams: URLSearchParams {
  'page' => '3',
  'limit' => '10',
  'category' => 'nodejs',
  'category' => 'javascript' }
searchParams.getAll(): [ 'nodejs', 'javascript' ] # key가 같으면 배열로 들어감
searchParams.get(): 10
searchParams.has(): true
searchParams.keys(): URLSearchParams Iterator { 'page', 'limit', 'category', 'category' }
searchParams.values(): URLSearchParams Iterator { '3', '10', 'nodejs', 'javascript' }
[ 'es3', 'es5' ]
[ 'es6' ]
[]
searchParams.toString(): page=3&limit=10&category=nodejs&category=javascript
```