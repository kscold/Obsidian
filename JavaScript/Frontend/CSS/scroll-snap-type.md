 - CSS Scroll snap은 웹 페이지에서 스크롤을 할 때, 요소가 스크롤되는 위치에 자동으로 스냅되도록 하는 CSS 속성이다.

- 예를들어 용자가 웹 페이지를 스크롤할 때 중간에 멈춰버리면 콘텐츠의 중간에서 멈춰 주요 콘텐츠의 일부만 보이게 되는데, 이를 미리 설정한 위치로 자동 스냅하여 자연스러운 스크롤 움직임과 함께 사용자 경험을 더욱 향상시키는 것이다.

- 스크롤 스냅이 없던 시절에는 자바스크립트로 일일히 요소 위치를 계산해서 조정해야 됬지만, CSS 속성을 통해 공식 지원함으로써 개발의 편리함과 더불어 성능도 비약적으로 상승했다고 볼 수 있다.

- scroll-snap-type을 사용하지 않은 예시이다.

![Scroll-Snap](https://blog.kakaocdn.net/dn/kZxZ9/btrhTgYWtDH/tBjCjFZXjuoLnPMENIIrI1/img.gif)

- scroll-snap-type을 사용한 예시이다.

![Scroll-Snap](https://blog.kakaocdn.net/dn/pQsUX/btrhTcPTWFl/9gskVusq6AM3a7wF8GlH1k/img.gif)


## 예시

- 우선 스크롤 스냅을 적용할 부모 컨테이너 영역을 만들고, 그 안에 스냅될 자식 요소 영역을 만들어 준다.
- 그리고 부모에 scroll-snap-type 속성과 자식에 scroll-snap-align 속성을 적용해주면 된다.

```html
<div class="scroll-container">
	<div class="scroll-area">1</div>
	<div class="scroll-area">2</div>
	<div class="scroll-area">3</div>
	<div class="scroll-area">4</div>
</div>
```

```css
/* 부모 스크롤 스냅 컨테이너 */
.scroll-container {
  overflow: auto;
  scroll-snap-type: y mandatory; /* y 축 방향으로만 scroll snap 적용 */
}

/* 자식 스크롤 스냅 영역 */
.scroll-area {
  scroll-snap-align: start; /* 스크롤 위치 맞춤 */
}

/* 밑에는 단순 꾸미기 */
.scroll-area:nth-of-type(1) {
  background: #49b293;
}

.scroll-area:nth-of-type(2) {
  background: #c94e4b;
}

.scroll-area:nth-of-type(3) {
  background: #4cc1be;
}

.scroll-area:nth-of-type(4) {
  background: #8360A6;
}
```
