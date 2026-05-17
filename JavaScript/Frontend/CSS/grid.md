- Grid는 카드형식으로 되어있는 신문형식의 레이아웃을 쉽게 만들수있고 여백을 쉽게 조절할수 있다.
- 시작은 flex-container와 비슷하게 [[display]]: grid를 설정해야한다.

- grid는 [[flex]]와 row, column 방향이 반대다.

- grid는 flex와 다르게 방향을 설정하지않고 각 아이템 크기를 설정할수있는데 설정하는방법은 밑의 예시와 같이 [[flex]]와는 다르게 한줄이 아닌 두줄을 설정할수 있다.

```css
/* 가로 : 한줄 설정해놓으면 다음줄도 똑같이 설정 */
grid-template-columns: 200px 200px 500px
ㄴ 박스 3개 width를 각 크기만큼 설정. 총페이지 width 900px 차지
grid-template-columns: 1fr 1fr 1fr
ㄴ 박스 3개를 한페이지 3분의1씩 할당.
grid-template-columns: repeat(3, 1fr)
ㄴ 위의 줄 내용과 같음.
grid-template-columns: 200px 1fr
ㄴ 박스 2개를 하나는 200px 하나는 나머지 차지.
grid-template-columns: 100px 200px auto
ㄴ 박스 2개를 100px 200px 하나는  나머지 부분에 할당.

/* 세로: 가로와 마찬가지 */
grid-template-rows: 200px 200px 500px
grid-template-rows: 1fr 1fr 1fr 
grid-template-rows: repeat(3, 1fr) 
grid-template-rows: 200px 1fr 
grid-template-rows: 100px 200px auto 
```

## 속성

```css
grid-template-rows: 1fr 1fr 1fr 1fr;
```
### repeat

- 위의 코드를 4번 적기보다는 repeat 함수를 쓰면 밑에와 같이 간단하게 적을 수 있다.

```css
grid-template-rows: repeat(4,1fr)
```
### min-max

- min-max는 최소크기와 최대 크기를 정하는 것인데 밑의 예시와 같이 적으면 높이가 100px로 고정되지만 텍스트가 길어지면 그 박스만 자유롭게 늘어난다.

```css
grid-template-rows: repeat(3, minmax(100px, auto))
```

### auto-fit, auto-fill

- auto-fill은 밑에와 같이 설정하면 25퍼센트로 첫줄에 4개씩 정렬이 된다.
- 총 아이템이 11개라고 가정하면 맨 밑줄에 3개가 남게 되는데 auto-fill은 채워는 주지만 맨밑에 3개를 정렬하고 빈공간으로 남게된다.

- 여기서 auto-fit으로 수정해주면 맨 밑줄3개가 늘어나 한줄을 꽉 채우게 된다.

```css
grid-template-columns: repeat(auto-fill, minmax(25%,auto))
```

## gap

- 사이공간을 조정한다.
- row-gap, column-gap 이렇게 각 방향별 설정도 가능하고 gap: 10px 이런식으로 줄여서도 사용가능하다.

```css
gap: 10px 20px;  
row-gap: 10px; 
column-gap: 20px;
```

- 이렇게 가로세로도 한번에 사용가능하다.

### 각셀의 영역 지정

- 아래 그림과 같이 각 아이템별로 영역 지정가능하다.

![](https://velog.velcdn.com/images/ikkim01/post/56cb9fba-31c1-4947-956b-9bb537e8a511/image.png)  
### 영역이름으로 지정

![](https://velog.velcdn.com/images/ikkim01/post/cb62e40f-81fe-4bc7-b152-62f53ba8548a/image.png)  

- 이런식으로 설정하고 싶으면 각 클래스 부분에 이런식으로 영역을 설정 해주면 된다.  

```css
.header { grid-area: header; }  
.sidebar-a { grid-area: a; }  
.main-content { grid-area: main; }  
.sidebar-b { grid-area: b; }  
.footer { grid-area: footer; }
```