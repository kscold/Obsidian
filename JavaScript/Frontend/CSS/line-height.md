
- line-height는 line-box의 높이를 설정하는 CSS 속성이다.  
- 일반적으로 텍스트 줄 사이의 거리를 설정하는데 사용된다.(자간이라고 생각하면 된다.)

![](https://velog.velcdn.com/images%2Fursr0706%2Fpost%2F9e375b34-74e4-4231-9d86-84e67beeaa09%2F%EC%BA%A1%EC%B2%981.PNG)

h1과 p태그를 사용하여 입력한 text이다.  
기본값으로 설정되어있는 텍스트에 line-height속성을 넣기 전 모습이다.

![](https://velog.velcdn.com/images%2Fursr0706%2Fpost%2F2fc39eda-6d7b-4555-85b7-de5a80af66a9%2F%EC%BA%A1%EC%B2%982.PNG)

`line-height: 30px;` 의 속성값을 입력 했더니 줄 간격이 멀어졌다.  
그러나 line-height 입력값의 단위로 px(픽셀)은 적합하지 않다.

![](https://velog.velcdn.com/images%2Fursr0706%2Fpost%2F85ff70c0-80ec-4467-89e0-cf8bb883ff19%2F%EC%BA%A1%EC%B2%983.PNG)

line-height값은 font사이즈에 맞게 커지고 줄어들어야 보기에 자연스러운데,  
값을 px단위로 설정하면 값이 그 px크기만큼으로 고정되어 font사이즈를 수정하게되면 너무 좁거나 너무 넓게 된다.  
그래서 보통 `line-height: 1.5;` 와 같은 식으로 상대적인 값으로 입력한다.

## line-height값 입력시 주의사항

간혹 line-hiehgt의 값으로 em단위를 쓰는 경우도 있다.  
**단위를 붙이지 않은 값과 em단위를 붙인 값은 서로 다른 결과를 부른다.**

![](https://velog.velcdn.com/images%2Fursr0706%2Fpost%2F34df5bb2-51ec-45f9-8cd2-6bb20e9cc4c8%2F%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202020-07-26%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%203.01.21.png)

```css
.parent {
font-size: 20px;
line-height: 1;
}

. first {
font-size: 30px;
}

.second {
font-size: 40px;
}
```

먼저 부모요소의 font-size값이 20px인 상태에서 단위를 붙이지 않고 `line-height: 1;`을 적용한 모습이다.  
그 속성값이 상속되어 자식요소인 first와 second클래스에 부여되는데 각각의 font-size에 맞게 line-height가 설정된 것을 볼 수 있다.  
그렇다면 em단위를 사용하면 어떻게 다를까?

![](https://velog.velcdn.com/images%2Fursr0706%2Fpost%2F1ca4d8c4-10fa-4ff9-94fe-e3f456514cd3%2F%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202020-07-26%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%203.13.00.png)

줄 간격이 엉망으로 바뀌었다.  
parent의 font-size가 20px로 되어있는데, `line-height:1em;`으로 값을 주게되면 font-size인 20px의 1배. 즉, 20px만큼의 line-height가 적용되고, 그 20px의 line-height값이 그대로 상속되어 자식요소의 font-size에 상관 없이 각각 20px의 line-height값이 적용된다.  
때문에 해당 font-size에 맞지 않은 line-height를 갖게 되어 이상하게 보이는 것이다.  
**line-height값은 상속을 고려하여 단위를 빼고 쓰자.**
## line-height영역

![](https://velog.velcdn.com/images%2Fursr0706%2Fpost%2F543aafa0-5478-4f9d-92a2-b60976be887f%2F%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202020-07-26%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%203.53.22.png)

text의 위 아래에는 leading영역이라는게 나눠져 있다.  
(leading영역이 있어야 text줄 위아래로 공간이 생긴다)  
font-size와 leading영역을 포함한 위아래 높이가 line-height가 된다.  
font종류에 따라 leading영역이 다르게 설정되어 있는데, line-height의 값이 기본값인 normal이라면 font종류에 따라 line-height도 달라지게 된다.  
따라서 normal로 두기 보다는 값을 명시적으로 입력해주는 것이 좋다.

![](https://velog.velcdn.com/images%2Fursr0706%2Fpost%2F3857084e-05f0-4621-90a8-036b863f5ccf%2F%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202020-07-26%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%204.10.56.png)

line-height값을 높이면 font크기는 그대로에leading영역이 위아래로 넓어진다.

### line-height: noraml

![](https://velog.velcdn.com/images%2Fursr0706%2Fpost%2F44dbc0bd-f78c-44da-bca6-42b4eb7db34e%2F%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202020-07-26%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%204.31.57.png)

```css
h2 {
font-size: 30px;
line-height: normal;
}
.one {
font-family: fantasy;
}
.two {
font-family: Arial Narrow;
}
.three {
font-family: cursive;
}
```

각각의 h2요소의 font-size는 30px로 통일하고 text-family만 다르게 적용했다.  
그런데 font-size가 동일하게 30px인데도 불구하고 line-height가 조금씩 다른걸 볼 수 있다.  
이유는 앞서 살펴봤듯이 font종류에 따라 leading영역이 다르게 설정되어 있는데, line-height값을 따로 명시하지 않고 normal로 두면 제각각 line-height가 다르게 나타나기 때문이다.

### line-height값 설정

![](https://velog.velcdn.com/images%2Fursr0706%2Fpost%2F38fcc825-e468-42ec-9119-cedc3e6d7332%2F%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202020-07-26%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%204.38.37.png)

```css
h2 {
line-height: 1.5;
}
```

line-height의 값을 특정한 값으로 설정해놓으면 위 그림과 같이 각각 font의 종류가 달라도 line-height가 균등하게 나타나는 것을 확인할 수 있다.