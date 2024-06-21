- 교차 관찰자(IntersectionObserver) API를 사용하여 페이지 스크롤 시 이미지를 Lazy-loading(지연 로딩)할 때나 Infinite scrolling(무한 스크롤)을 통해 스크롤할 때 새로운 콘텐츠를 불러올 때 등을 구현하기 쉽다.

- [[비동기(asynchronous)]]적으로 실행되기 때문에 메인 스레드에 영향을 주지 않으면서 변경 사항을 관찰할 수 있다.

- 또한 IntersectionObserverEntry의 [[속성(Property)]]을 활용하면 getBoundingClientRect()를 호출한 것과 같은 결과를 알 수 있기 때문에 따로 getBoundingClientRect() 함수를 호출할 필요가 없어 리플로우 현상을 방지할 수 있다.


## IntersectionObserver의 사용 예시

```js
const count = 20; // 한페이지 당 보여줄 최대 요소의 갯수
let itemIndex = 0; // 시작 요소 인덱스

// IntersectionObserver 객체를 생성해 등록함
const observer = new IntersectionObserver(
	(entries) => {
		console.log('entries', entries);
		
		entries.forEach((entry) => {
			// 관찰 대상이 viewport 안에 들어온 경우 'list' 클래스를 추가
			const list = document.querySelector('.list');
			
			if (entry.isIntersecting) { // 관찰 되었으면
				
				for (let i = itemIndex; i < itemIndex + count; i++) {
					let item = document.createElement('p');
					item.textContent = i;
					item.className += 'item';
					list.appendChild(item);
				}	
				
				itemIndex = itemIndex + count;
			}
		});
	},{ root: null, threshold: 0.1 } // 10%만 교차를 해도 호출이 됨
);

// 관찰할 대상을 선언하고, 해당 속성을 관찰시킴
observer.observe(document.querySelector('.end')); // 감시할 태그를 선택

// 뷰 포트 안에 있으면 isIntersecting이 true 즉, intersectionRatio: 1일 때
```