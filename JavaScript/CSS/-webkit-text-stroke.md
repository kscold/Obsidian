
- `-webkit-text-stroke`는 웹킷(Webkit) 엔진을 사용하는 브라우저에서 텍스트의 테두리를 그리는 CSS 속성이다.
- 이 속성을 사용하면 텍스트 주위에 선을 그릴 수 있으며, 이것은 주로 텍스트를 강조하거나 스타일링할 때 사용된다.

이 속성은 다음과 같은 구문을 가집니다:

```css
-webkit-text-stroke: <width> <color>;
```

- `<width>`: 테두리의 너비를 나타내는 값이다. 양수 또는 음수 값이 될 수 있다. 양수 값은 텍스트 주위에 테두리를 생성하고, 음수 값은 텍스트 내부에 테두리를 생성합니다. 값의 단위는 텍스트 크기의 비율이며, 기본값은 0이며, 이 경우 테두리가 그려지지 않는다.
    
- `<color>`: 테두리의 색상을 지정하는 값입니다. CSS에서 사용하는 색상 값인 키워드, HEX, RGB 등을 사용할 수 있습니다.
    

예를 들어, 텍스트에 얇은 검은 테두리를 추가하려면 다음과 같이 사용할 수 있습니다:

```css
-webkit-text-stroke: 1px black;
```

그러나 주의해야 할 점은 `-webkit-text-stroke`가 웹킷 엔진에만 적용되며, 모든 브라우저에서 지원되지 않을 수 있다. 다른 브라우저에서도 호환성을 유지하려면 추가로 다른 방식의 텍스트 스타일링도 고려해야 한다.