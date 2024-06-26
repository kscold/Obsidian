- Query는 클라이언트 측 [[HTML(Hyper Text Markup Language)]] 스크립팅을 간소화하기 위해 고안된 크로스 플랫폼 자바스크립트 라이브러리다. 
- jQuery는 빠르고, 작고, 기능이 풍부한 자바스크립트 라이브러리이다.
- [[웹(Web)]] 상에서 가장 트래픽이 많은 상위 천만 개의 사이트 중 65%에서 사용 중이다.

- jQuery는 MIT 라이선스로 배포되는 무료 오픈소스 소프트웨어이다.

- jQuery는 여러 브라우저에서 동작하는 사용하기 쉬운 API를 통해 [[HTML(Hyper Text Markup Language)]] 문서 탐색과 조작, [[이벤트(event)]] 처리, 애니메이션, [[Ajax(Asynchronous JavaScript and XML)]] 등을 훨씬 더 간단하게 만들어준다.


## HTML 문서 탐색

- 브라우저가 웹 페이지를 렌더링할 때 웹 페이지는 [[DOM(Document Object Model)]]이라는 시각적 표현이 된다.
- 이 모델은 루트와 리프가 있는 특정 노드로 구성된 트리 자료구조이다.


![An example of the DOM for a basic web page](https://cms-assets.tutsplus.com/cdn-cgi/image/width=600/uploads/users/34/posts/26232/image/dom_tree.gif)

- jQuery를 이용하면 DOM의 내용을 쉽게 탐색함으로써 검색할 노드나 요소, 값에 접근하거나 찾을 수 있다.
- 즉, 고유 ID를 가진 [[<div>]] 요소의 텍스트를 찾는 다면 밑의 코드와 같이 접근할 수 있다.

```js
/**                                                            
* This code looks for a div with the ID of "my unique element" 
* and then removes it from view.                               
*/

$('div#my-unique-element').hide();
```

- [[<span>]] 요소를 모두 순회하는 경우에도 다음과 같이 처리할 수 있다.

```js
/**                                                        
* This is the basic way to setup a loop in jQuery. It will
* take all of the span elements on the page and then      
* allow you to iterate through them.                       
*/                                                         

$('span').each(function() {                              
	// Process the span element here                           
});                                                        
```

- 아래 코드는 메서드 체인을 사용하는 예시이다.

```js
$excerpt.on('keydown', function(evt) {
	if ((17 === evt.keyCode || 91 === evt.keyCode ) || 86 === evt.keyCode) {
		if ( -1 === $.inArray( evt.keyCode, keymap ) ) {
			keymap.push( evt.keyCode );
		}
	}
}).on('keyup', function() {
	if (user_has_pasted_content(keymap)) {
		resize_textarea( this );
		keymap = [];
	}
});
```

 - 위의 코드를 통해 메서드 체인을 사용해 특정 상황에서 jQuery가 얼마나 유용한지 확인할 수 있다.
 - 즉, jQuery의 위력은 [[DOM(Document Object Model)]]을 쿼리하고 API를 사용해 조작할 수 있다는 데 있다

## HTML 문서 조작

- 실제로 DOM을 조작할 때 jQuery에는 클라이언트가 보는 것을 변경할 수 있다.

- 이러한 함수 가운데 어떤 함수는 페이지가 로드될 때마다 표시되지 않는 요소를 표시(`show`)하거나 숨길(`hide`) 수 있게 하는 동작이나 새로운 요소를 생성하고 기존 요소 끝에 덧붙이거나(`append`) 전체 리스트 앞에 추가(`prepend`)할 수 있게 해준다.

- class 속성을 추가하기 위해 모든 `span` 요소를 순회하려고 한다면 다음과 같이 할 수 있다.

```js
/**                                                        
* This is the basic way to setup a loop in jQuery. It will 
* take all of the span elements on the page and then       
* add a custom class attribute to them.                    
*/                                                         

$('span').each(function() {                              
	$(this).addClass('my-custom-class'); 
});                                                        
```

- jQuery는 아래와 같은 것들을 할 수 있다.
	- 문서나 창, 특정 요소의 높이나 너비를 파악
	- 특정 요소의 값 획득(해당 요소가 값을 제공할 경우)
	- 클래스명 토글
	- 기타 등등

## 이벤트 처리

- 클라이언트가 요소를 클릭하거나, 키 입력을 하거나, 마우스를 클릭하면 브라우저는 발생한 이벤트에 해당하는 신호를 발생시킨다.
-  jQuery는 모든 이벤트에 대해 일관된 이름을 정의함으로써 이를 훨씬 쉽게 처리할 수 있다.

- 이를 통해 사용자와 브라우저의 상호작용을 이용할 수 있다.
- 즉, 응답하려고 하는 이벤트에 대해 동일한 이름을 사용할 수 있고 이러한 이름은 모든 주요 브라우저에서 통한다.
