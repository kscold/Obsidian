- CSS에서 `position` 속성은 HTML 문서 상에서 요소가 배치되는 방식을 결정한다.
- 많은 경우, `position` 속성은 요소의 정확한 위치 지정을 위해서 `top`, `left`, `bottom`, `right` 속성과 함께 사용된다.

## position: static

- `position` 속성을 별도로 지정해주지 않으면 기본값인 `static`이 적용된다.
- `position` 속성이 `static`인 요소는 HTML 문서 상에서 원래 있어야하는 위치에 배치된다.

- 이 말은 요소들이 HTML에 작성된 순서 그대로 브라우저 화면에 표시가 된다는 것을 뜻한다.
- 따라서 `top`, `left`, `bottom`, `right` 속성값은 `position` 속성이 `static`일 때는 무시된다.

- 예를 들어, 다음과 같이 `<main>` 요소 아래에 3개의 `<div>` 요소가 있다면 맨 위에 첫 번째 요소, 중간에 두 번째 요소, 제일 아래에 세 번째 요소가 나란히 순서대로 배치된다.

```html
<main>
  <div>첫 번째 요소</div>
  <div>두 번째 요소</div>
  <div>세 번째 요소</div>
</main>
```

- 시각적으로 비교가 용이하도록 두 번째 요소의 배경색을 투명도를 다르게 스타일하였다.

```css
main {
  width: 300px;
  height: 400px;
  background: tomato;
}

div {
  width: 200px;
  height: 100px;
  border: 1px solid;
  background: yellow;
  text-align: center;
  line-height: 100px;
  box-sizing: border-box; // 태두리를 포함한 넓이로 설정
}

div:nth-of-type(2) {
  position: static;
  background: cyan; // 파란색으로 설정
  opacity: 0.8; // 투명도 설정
}
```

- 코드 실행결과는 다음과 같다.

![[Pasted image 20240115011622.png]]
## position: relative

- `position` 속성을 `relative`로 설정하게 되면, 요소를 원래 위치에서 벗어나게 배치할 수 있게 된다.
- 요소를 원래 위치를 기준으로 상대적(relative)으로 배치해준다고 생각하시면 이해가 쉽다.
- 요소의 위치 지정은 `top`, `bottom`, `left`, `right` 속성을 이용해서, 요소가 원래 위치에 있을 때의 상하좌우로 부터 얼마나 떨어지게 할지를 지정할 수 있다.

- 예를 들어, 두 번째 `<div>` 요소의 `position` 속성을 `relative`로 변경하고, 요소의 원래 위치로 부터 위에서 `28px`, 왼쪽에서 `48px` 떨어지도록 `top`과 `left` 속성을 설정해본다.

```css
div:nth-of-type(2) {
  position: relative;  top: 28px;  left: 48px;  background: cyan;
  opacity: 0.8;
}
```

- 아래 보이는 것처럼 두 번째 요소의 아랫 부분이 세 번째 요소와 살짝 겹치도록 배치가 변경된 것을 확인할 수 있다.

![[Pasted image 20240115011602.png]]

## position: absolute

- `position` 속성값 중 가장 난해하고 주의해서 사용해야하는 속성값은 `absolute`이다. 
- `absolute`라는 영단어의 의미가 `relative` 속성의 정반대 개념이라고 많이 오해하기 쉽다.
- 오히려 `absolute` 속성값은 `relative` 속성값과 함께 사용되는 경우가 많다.

- `position` 속성을 `absolute`로 지정하면 사실 전혀 절대적(absolute)으로 요소를 배치해주지 않다. 
- 오히려 배치 기준이 상황에 따라 굉장히 달라질 수 있다.

- `position` 속성이 `absolute`일 때 해당 요소는 배치 기준을 자신이 아닌 상위 요소에서 찾는다. 
- DOM 트리를 따라 올라가다가 `position` 속성이 `static`이 아닌 첫 번째 상위 요소가 해당 요소의 배치 기준으로 설정된다. 
- 만약에 해당 요소 상위에 `position` 속성이 `static`이 아닌 요소가 없다면, DOM 트리에 최상위에 있는 `<body>` 요소가 배치 기준이 된다.

- 실제로 `absolute` 속성값을 사용할 때 이러한 복잡한 특성을 활용하는 경우는 드물고 대부분의 경우, 부모 요소(가장 가까운 상위 요소)를 기준으로 `top`, `left`, `bottom`, `right` 속성을 적용할 때 사용한다.
- 따라서 어떤 요소의 `position` 속성을 `absolute`로 설정하면, 부모 요소의 `position` 속성을 `relative`로 지정해주는 것이 관례이다.
- 이때 지정된 위치는 독립되어 다른 요소와 상호작용하지 않는다.

- 밑에 css 예시 코드에서 두 번째 `<div>` 요소의 부모인 `<main>` 요소의 `position` 속성을 `relative`로 변경한다.

```css
main {
  position: relative; /* 부모 코드를 relative로 설정함 */
  width: 300px;
  height: 400px;
  background: tomato;
}
```

- 그 다음 두 번째 `<div>` 요소의 `position` 속성을 `absolute`로 변경하고, 부모 요소를 기준으로 하단에서 `8px`, 우측에서 `16x` 떨어지도록 `bottom`과 `right` 속성을 설정해준다.

```css
div:nth-of-type(2) {
  position: absolute; /* 부모 코드를 기준으로 bottom과 right가 이동됨 */
  bottom: 8px;
  right: 16px;
  background: cyan;
  opacity: 0.8;
}
```

- 이제 두 번째 요소가 `<main>` 요소의 우측 하단에 배치된 것을 확인할 수 있다.

![[Pasted image 20240115012332.png]]

- 여기서 꼭 짚고 넘어가야하는 부분이 있다.
- 바로 `position: absolute`인 요소는 HTML 문서 상에서 독립되어 앞뒤에 나온 요소와 더 이상 상호작용을 하지 않게 된다는 것이다. 
- 따라서 위에서 보이는 것처럼, 첫 번째 요소 아래에 바로 세 번째 요소가 배치되었다.

## position: fixed

- 화면을 위아래로 스크롤하더라도 브라우저 화면의 특정 부분에 고정되어 움직이지 않는 UI를 만들 때 주로 사용한다.(보통 라이브 채팅 버튼을 구현할 때 많이 쓰이는 기법이다.)
- `position` 속성을 `fixed`로 지정하면 이렇게 요소를 항상 고정된(fixed) 위치에 배치할 수 있다.

- `fixed` 속성값의 배치 기준이 자신이나 부모 요소가 아닌 뷰포트(viewport), 즉 브라우저 전체화면이기 때문이다.
- `top`, `left`, `bottom`, `right` 속성은 각각 브라우저 상단, 좌측, 하단, 우측으로 부터 해당 요소가 얼마나 떨어져있는지를 결정한다.

- 두번째 `<div>` 요소의 `position` 속성을 `fixed`로 변경하고, 뷰포트 기준으로 하단에서 `8px`, 우측에서 `16x` 떨어지도록 `bottom`과 `right` 속성을 설정해준다.

```css
div:nth-of-type(2) {
  position: fixed;  bottom: 8px;  right: 16px;  background: cyan;
  opacity: 0.8;
}
```

- 이제 두번째 요소가 부모인 `<main>` 요소를 벗어나 브라우저 전체화면 기준으로 우측 하단에 배치되는 것을 볼 수 있다.

![[Pasted image 20240115012629.png]]

- `position: fixed`인 요소도 `position: absolute`인 요소와 마찬가지로 HTML 문서 상에서 독립되어 앞뒤에 나온 요소와 더 이상 상호작용을 하지 않는다.

## position: sticky

- `position` 속성의 `sticky` 값은 CSS에서 비교적 최근에 추가된 속성이다.
- 특이하게도 브라우저 화면을 스크롤링할 때 효과가 나타난다.

- 먼저, `<div>` 요소의 부모인 `<main>` 요소의 높이를 줄이고 스크롤링이 가능해지도록 `height`외 `overflow` 속성을 조정해준다.

```css
main {
  width: 300px;
  height: 120px;  overflow: auto;  background: tomato;
}
```

- 그 다음, 두번째 `<div>` 요소의 `position` 속성을 `sticky`로 변경하고, `top` 속성을 `0`으로 설정해준다.

```css
div:nth-of-type(2) {
  position: sticky;  top: 0;  background: cyan;
  opacity: 0.8;
}
```

![[Pasted image 20240115012822.png]]

- 이제 스크롤바를 아래로 내려서 화면을 위로 올려보면, 두 번째 요소가 화면 상단에 끈적하게(sticky) 붙어서 움직이지 않는 것을 알 수 있다. 
- 반면에 `position: static`인 세 번째 요소는 이에 구애받지 않고 화면에 따라 올라가는 것을 알 수 있다.



