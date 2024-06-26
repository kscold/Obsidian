- border-bottom CSS 단축 속성은 요소의 아래쪽 테두리를 설정한다.
- border-bottom-width,border-bottom-style, border-bottom-color의 값을 지정한다.

- 다른 단축 속성과 마찬가지로, `border-bottom`는 자신이 포함한 모든 값을 지정하며 사용자가 명시하지 않은 속성도 기본값으로 설정한다.


## 문법


```css
border-bottom: 1px;
border-bottom: 2px dotted;
border-bottom: medium dashed green;
```

- border-bottom은 한 개에서 세 개의 값을 사용해 지정할 수 있고, 순서는 상관하지 않는다.

- 즉, 아래 두 코드는 사실 동일하다.

```css
border-bottom-style: dotted;
border-bottom: thick green;
```

```css
border-bottom-style: dotted;
border-bottom: none thick green;
```

- 따라서 border-bottom보다 먼저 지정한 border-bottom-style의 값은 무시된다. 
- border-bottom-style의 기본값은 none이므로, border-style을 명시하지 않으면 테두리를 만들지 않는다.