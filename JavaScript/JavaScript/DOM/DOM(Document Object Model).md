- [[객체(Object)]] 형식으로 문서 구조를 표현하는 방법으로 XML이나 [[HTML(Hyper Text Markup Language)]]로 작성한다.

- 즉, DOM은 문서 객체한 HTML, head, body와 같은 태그들을 자바스크립트가 이용할 수 있도록 만든 [[객체(Object)]]를 의미한다.

- DOM은 트리 형태라서 특정 노드르 찾거나 수정, 제거, 원하는 위치에 삽입이 가능하다.


## DOM의 역할

- 브라우저가 [[HTML(Hyper Text Markup Language)]] 코드를 분석해서 트리로 만들고 API들을 지원을 하기 때문에 이 API들을 이용하여 DOM을 조작할 수 있게 만들어 준다.

![[Pasted image 20240101213717.png]]

- 예를 들어 DOM 안에 [[클래스(Class)]]가 버튼이라는 이름을 가진 요소에 접근한다면 document.querySelector와 같은 [[window]] [[객체(Object)]]의 기능을 이용하여 접근을 할 수 있는 것이다.
- 또한 document.querySelector와 같은 코드는 자바스크립트 안에 있지만 실제 자바스크립트 코드는 아니다.

- DOM이 존재하기 때문에 자바스크립트는 [[HTML(Hyper Text Markup Language)]] 태그들을 수정할 수 있는 것이다.


## [[window]] [[객체(Object)]]에서의 DOM 

- [[Web API]]의 시리즈인 [[BOM(Browser Object Model)]] 안에 window.location, window.navigator, window.history, window.screen 등이 있는데 그 중에서 [[window.document]]를 DOM이라고 부른다.

![[Pasted image 20240430002056.png]]


## DOM의 문제점

- DOM API를 사용하는 서비스들은 동적 UI에 최적화되어 있지 않다는 문제가 있다. HTML 자체는 정적이므로 자바스크립트를 이용해 동적으로 만들 수 있다.

- 따라서 DOM은 트리 구조로 되어 있어서 이해하기 쉽지만, 노드의 수가 많아질 수록 속도가 느려지고, DOM 업데이트에 잦은 오류를 발생시킬 수 있다.
- 또한, 최근 모던 [[웹(web)]]은 [[SPA(Single Page Application)]]을 사용한다. 
- 하나의 웹 페이지를 어플리케이션처럼 구성하는 [[SPA(Single Page Application)]]에서는 [[HTML(Hyper Text Markup Language)]] 문서 자체가 하나이며, 여러 동적인 기능이 들어가기 때문에 안그래도 리소스가 모두 합쳐진 무거운 [[HTML(Hyper Text Markup Language)]] 문서를 지속적으로 재랜더링 해줘야한다는 문제점이 발생하게 되었다.

- 즉, DOM을 업데이트 하는 것은 콘텐츠 변경을 포함할 뿐만이 아니라 훨씬 더 많은 작업들이 요구된다. 
- 또한 CSS를 다시 계산하고 레이아웃을 변경하려면 복잡한 알고리즘이 필요하며 성능에 영향을 미친다. 
- 이처럼 기존에는 화면의 변경사항을 DOM을 직접 조작하여 브라우저에 반영하였다.
- 하지만, 이 방법의 가장 큰 단점은 DOM 트리가 수정될 때마다 렌더 트리가 계속해서 실시간으로 갱신된다는 점이다.

- 예를 들어 화면에서 10개의 수정사항이 발생하면 수정할 때마다 새로운 랜더 트리가 10번 수정되면서 새롭게 만들어지게 되는 것이다.
- 페이스북을 예시로만 들어도 div의 갯수가 천문학적으로 커지기 때문에 규모가 큰 웹이 DOM에 직접 접근하여 변화를 주다 보면 성능이슈가 발생한다.
- DOM 자체는 빠르지만 DOM에 변화가 일어나면 웹 브라우저가 CSS를 다시 연산하고, 레이아웃을 구성하기 때문에 이 과정에서 시간이 허비가 된다.

- 따라서 [[리액트(React)]]는 [[Virtual DOM]]을 사용하여 실제 DOM에 접근하여 조작하는 대신 이를 추상화한 자바스크립트 객체를 구성하여 사용한다.


## DOM vs Virtual DOM
|                     | DOM                                 | Virtual DOM                                                          |
| ------------------- | ----------------------------------- | -------------------------------------------------------------------- |
| 업데이트                | 느리다.                                | 빠르다.                                                                 |
| HTML 업데이트 방식        | HTML을 직접적으로 업데이트한다.                 | HTML을 직접적으로 업데이트 하지 않는다.                                             |
| 새로운 element 업데이트 방식 | 새로운 element가 업데이트된 경우 새로운 DOM 생성한다. | 새로운 element가 업데이트 된 경우 새로운 가상 DOM 생성 후 이전 DOM과 비교 후 차이점 DOM만 업데이트한다. |
| 메모리                 | 메모리 낭비가 심하다.                        | 메모리 낭비가 덜심하다.                                                        |

### DOM코드

```html
<ul class="fruits">
    <li>Apple</li>
    <li>Orange</li>
    <li>Banana</li>
</ul>
```

### Virtual DOM식으로 표현된 코드

```js
// Virtual DOM representation
{
  type: "ul",
  props: {
    "class": "fruits"
  },
  children: [
    {
      type: "li",
      props: null,
      children: [
        "Apple"
      ]
    },
    {
      type: "li",
      props: null,
      children: [
        "Orange"
      ]
    },
    {
      type: "li",
      props: null,
      children: [
        "Banana"
      ]
    }
  ]
}
```

