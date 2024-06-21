- 자바스크립트로 [[HTML(Hyper Text Markup Language)]] 태그를 생성할 수 있다.
- createElement [[메서드(Method)]] 태그 이름을 넣어 요소 생성할 수 있다.


## crrateElement 태그의 attribute 추가 방법

- crrateElement()로 [[인스턴스(Instance)]]를 만들어서 속성(attribute)을 추가하는 방법이다.

```js
// element로 li 태그 생성
const li = document.createElement('li');

// li 태그 class 속성 추가하기
li.className = 'list-group-item';

// id 속성 추가하기
li.id = 'new-item';

// name 속성을 추가
li.setAttribute('name', 'New list item'); 
```


## appendChild()

- appendChild() [[메서드(Method)]]는 자바스크립트 Node 인터페이스의 [[메서드(Method)]]이다.
- 지정된 부모 노드에서 appendChild() [[메서드(Method)]]를 사용하면 부모 노드가 가지고있는 자식 노드 목록의 끝에 [[매개변수(parameter)]]로 전달 받은 노드를 추가한다.

- appendChild [[메서드(Method)]]는 append 메서드와는 다르게 오직 Node 객체만 받을 수 있다.
- append는 여러 개의 노드와 문자를 추가할 수 있는 반면에 appendChild 메서드는 한번에 오직 하나의 노드만 추가할 수 있다.

```js
// Node object 삽입
const parent = document.createElement('div');
const child = document.createElement('p');

parent.appendChild(child);

// <div><p></p></div>
```