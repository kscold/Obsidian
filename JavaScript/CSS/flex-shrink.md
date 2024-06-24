- [[flex-grow]]와 반대로, flex-item([[flex]]의 자식요소)의 너비를 감소시키는 비율을 설정한다. 
- 초기값은 1이고 상속받지 않는다.

- flex-shrink: 0; 줄어들지 않는다.(수축 가능성이 없다.)

- [[flex-basis]]로 크기를 정해두었어도 고정된게 아니기 때문에, 크기가 변할 수 있다.  
- 이때 flex-shrink: 0을 선언하면 원하는 크기대로 고정할 수 있다.


## 예시

- flow-shrink 속성이 레이아웃을 벗어난 아이템 너비를 분배해서 줄이는 방법이다.
- flex-shrink 속성은 플렉스박스에 [[flex-wrap]]: wrap; 속성을 부여한 경우 적용되지 않다.

- [[flex-wrap]] 속성을 정의하지 않거나(기본 값 "nowrap") [[flex-wrap]]: nowrap; 속성을 부여해야 한다.

- 레이아웃 너비를 넘을 경우 끝에 걸치는 아이템은 다음 행의 왼쪽부터 다시 배치되기 때문에 레이아웃 영역을 넘는 상황이 생기지 않는다.

- 그리고 "flex-shrink" 속성은 기본 값이 "1"이기 때문에 속성을 정의하지 않아도 자동으로 아이템이 축소되어 적용된다는 것을 염두해야 한다.

- 자동으로 아이템 너비가 축소되지 않도록 하려면 반드시 "flex-shrink: 0;"을 아이템에 선언해야 한다.

- 아이템들의 너비 합이 레이아웃 너비보다 넓은 다음 예를 사용해 레아웃을 넘어간 아이템 영역을 분배해서 줄인다.

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

- 플렉스박스의 너비는 "600px" 이고 아이템 너비는 "200px"이다.
- "flex-shrink: 0;" 속성을 아이템에 부여해 아이템 자동 축소를 강제로 막았다.

```css
.layout{
    max-width: 600px;
    margin: 0 auto;
    padding: 0;
}

.flexbox{
    display: flex;
    flex-wrap: nowrap;
    gap: 0;
    padding: 10px;
    background-color: #f0f0f0;
}

.item{
    min-height: 150px;
    flex-basis: 200px;
    flex-shrink: 0;
}
```

![IMG_398](https://apost.dev/content/images/2023/12/img_3-98.jpg)

- 레이아웃 영역을 넘어선 아이템들에 "flex-shrink: 1;" 속성을 부여하면 아이템들 너비가 똑같이 줄어들어 다음처럼 아이템 너비가 모두 "145px" 가  된다.( 145px x 4 = 580px + 플렉스박스 패딩 10px x 2 = 600px)

- 아이템들에 "flex-shrink" 속성을 다음처럼 각각 부여해서 남는 아이템 너비가 어떻게 분배 축소되는지 확인해본다.

```css
.item:nth-child(1){flex-shrink: 1;}
.item:nth-child(2){flex-shrink: 0;}
.item:nth-child(3){flex-shrink: 1;}
.item:nth-child(4){flex-shrink: 2;}
```

- 아이템들의 "flex-shrink" 속성 값의 합은 4이다.
- 그리고 아이템이 플렉스박스에서 넘어간 너비는 "220px"이다.
- 패딩 공간도 넘어간 것 영역에 포함된다.

- 220px / 4 = 55px 가 되고 "55px" 가 "flex-shrink" 속성 값 1의 너비가 된다.
- 이제 속성 값 만큼씩 너비를 맞춰서 줄이면 다음처럼 아이템 영역이 정해진다.

- 각각의 너비는 순서대로 145px, 200px, 145px, 90px가 된다.

![IMG_475](https://apost.dev/content/images/2023/12/img_4-75.jpg)

- 축소 비율에 맞춰 너비가 줄어든 아이템들을  "flex-shrink" 속성의 적용 방식을 정리하면 다음과 같다.

![IMG_558](https://apost.dev/content/images/2023/12/img_5-58.jpg)