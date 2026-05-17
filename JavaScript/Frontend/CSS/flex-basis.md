- flex-item([[flex]]의 자식)의 초기 크기를 지정하는 속성이다.(원래 자기의 크기이다.)
- 초기값은 auto이고, 상속하지 않는다.

- flex-basis는 요소 크기를 강제하지 않는다.
- 값이 auto 일 때, 요소는 컨텐츠만큼의 영역을 가진다.

## flex-basis: 0과 auto의 차이점

```html
<div class="playground">
	<div class="frog">Frog</div>
	<div class="frog">Frog</div>
	<div class="frog">Frog</div>
</div>
```

```css
/* 설명에 불필요한 속성들은 제외함 */
.playground { display: flex; }
.frog 		{ flex-grow: 1; }
```

- 이 코드에서, 우리는 flex-basis 값에 따라 요소가 어떻게 변하는가에 대해 살펴볼 것이다.

### flex-basis: auto

- (기본값)일 때와 0일 때, 위 코드를 실행시켜보면 아래와 같은 결과가 나온다.  

![](https://velog.velcdn.com/images/930_10/post/c3a3c4eb-b1df-485a-9bc8-d334074b9ba5/image.png)

- 아래는 flex-basis: auto일 때, 요소 속 콘텐츠의 크기가 커졌을 때 결과이다.

![](https://velog.velcdn.com/images/930_10/post/f1a5e90d-9a71-4646-974a-35cd5539580b/image.png)

- auto일 때엔 flex-item의 기본 크기=자신의 콘텐츠만큼 가지게 된다.
### flex-basis: 0

![](https://velog.velcdn.com/images/930_10/post/ed846cdf-6cff-4405-b73b-be392f77ea7a/image.png)

- flex-basis가 0일 때엔, 애초에 아이템의 기본 크기를 0이라고 지정한 것이므로 원래 자기 콘텐츠 = 0 이다.  
- 따라서, 0 + 동등하게 나눠진 여백의 결과값으로 서로 동일한 너비를 가지게 되었다.

### [[flex-grow]] 

- 1에 따라 세 아이템 모두 같은 여백을 나눠가지지만, 원래의 자기 콘텐츠 + 나눠진 여백만큼을 자기가 가지므로 서로 다른 크기를 가지게 된 것이다.