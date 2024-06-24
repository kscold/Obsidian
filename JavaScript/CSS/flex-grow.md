- flex-item 요소([[flex]]의 자식)가 flex-container 요소([[flex]]의 부모) 속에서 차지한 영역(원래 자기 크기)을 제외한 사용가능한 공간을 비율로 분배하여 원래 자기 크기에 추가한다.

- flex-container의 남은 공간을 flex-item이 얼마만큼의 비율로 채울 수 있게 할지 결정하는 속성이다.
- 속성값은 숫자 단독으로만 사용한다.
- 비율로 분배하는 것이기 때문, 당연히 음수값은 사용하지 않는다.


## 문법

- 초기값은 0이고, 상속하지 않는다.


## 예시

- flex-grow 속성이 적용되지 않거나, 속성 값이 "0" 인 경우 레이아웃 너비보다 아이템들의 너비 합이 더 작으면 아이템 오른쪽 끝에 여백이 남게 된다.

- 다음과 같은 기본적인 플렉스박스에서 플렉스박스 아이템 CSS 에서 "flex-grow" 속성을 없애면 다음과 같이 된다.

```html
<div class="layout">
    <div class="flexbox">
        <div class="item">content1</div>
        <div class="item">content2</div>
        <div class="item">content3</div>
        <div class="item">content4</div>
    </div>
</div>
```


```css
.flexbox{
    display: flex;
    flex-wrap: nowrap;
    gap: 0;
    padding: 10px;
    background-color: #e8e8e8;
}

.item{
    min-height: 150px;
    flex-basis: 100px;
}
```

- 기본 너비가 "100px" 이기 때문에 기본 너비로 고정되면서 너비 "600px" 인 레이아웃 오른쪽에 여백이 남게 된다.

![IMG179](https://apost.dev/content/images/2023/12/img-179.jpg)


```css
.item:nth-child(1){flex-grow: 1;}
.item:nth-child(2){flex-grow: 1;}
.item:nth-child(3){flex-grow: 0;}
.item:nth-child(4){flex-grow: 2;}
```

- 오른쪽에 여백이 남는 플렉스박스 아이템들에 위의 코드처럼 "flex-grow" 속성을 부여하면 아래 이미지처럼 아이템들이 주어진 비율대로 늘어나면서 레이아웃 영역을 채우게 된다.

![IMG_1155](https://apost.dev/content/images/2023/12/img_1-155.jpg)

- 레이아웃에 아이템들이 채워진 플렉스박스 영역을 채우는 방식은 다음과 같다.
- 플렉스박스 안의 아이템들에 적용된 "flex-grow" 속성 값의 합을 구합니다. 앞서의 예는 4가 된다.

- 플렉스박스의 남는 여백을 4로 나눈다.
- 남는 공간의 여백은 "180px" 이고 180px / 4 = 45px 가 된다. 
- 실제 아이템이 차지하는 영역을 기준으로 하기 때문에 패딩 영역이 차지하는 20px는 제외된다.

- "flex-grow" 속성 값을 기준으로 비율 만큼씩 아이템 너비를 더한다.
- 원래의 너비였던 "100px" 에서 왼쪽 아이템부터 순서대로 45px, 45px, 0px, 90px 를 더하면 최종 너비가 된다.

- 플렉스박스 "flex-grow" 속성의 적용 방식을 정리하면 다음과 같다.

![IMG_2129](https://apost.dev/content/images/2023/12/img_2-129.jpg)

- flex-grow: 2의 의미는 자기 영역을 제외한 사용 가능한 공간을 2만큼 균등하게 분배하여 원래 자기 크기(basis 값)에 그 비율만큼 추가된다.