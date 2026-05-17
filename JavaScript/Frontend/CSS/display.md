- 요소를 어떻게 보여줄지 레이아웃을 결정하는 속성이다.
- 초기값은 inline이다.
- 모든 elements에 적용 가능하다.

## 1. display : none

 - 요소를 렌더링하지 않도록 설정하고 visibility 속성을 hidden으로 설정한 것과 달리 영역도 차지하지 않는다.
 
### 예시

```html
<style>
	.display-none{ display: none }
	.invisible{ visibility: hidden }
</style>

<div class="display-none">1</div>
<div>2</div>

<div class="invisible">3</div>
<div>4</div>
```

```bash
2
	// visibility: hidden를 설정한 것은 영역을 차지한다.
4
```

## 2. display : inline

- [[<span>]]태그, [[<a>]]태그, b태그, i태그, [[<img>]]태그 등이 해당한다.

- block과 달리 줄 바꿈이 되지 않고, width와 height, 여백을 지정할 수 없다.
- 문서에서 볼드, 이탤릭, 색상, 밑줄 등 글자나 문장에 효과를 주기 위해 존재하는 단위라고 할 수 있다.

### 3. display : block

- [[<div>]]태그, [[<p>]]태그, h태그, [[<li>]]태그, [[<form>]]태그등이 해당한다.

- 가로 영역을 모두 차지하며 항상 줄 바꿈이 되어 새로운 라인에서 시작한다. 
- 문서에서 문단을 표시할 때, 한 문단이 끝난 뒤에 나타나는 요소는 항상 다음 줄에 표시되던 것과 비슷한 맥락이다.  

- width와 height 속성을 지정할 수 있다.

## 4. display : inline-block

- 요소 자체는 inline요소처럼 동작하지만(줄바꿈이 되지 않는다.) 해당 요소 내부에서는 블록 요소처럼 동작한다.(크기나 여백을 지정할 수 있다.)

## 5. display : [[flex]]

- 아이템들을 가로 방향 혹은 세로 방향으로(1차원 배치) 배치할 수 있는 방식으로 요소의 크기가 불분명하거나 동적인 경우에도 각 요소를 정렬할 수 있는 효율적인 방법을 제공한다.  

- Flex의 속성은 컨테이너에 적용되는 속성, 아이템에 적용되는 속성으로 나뉜다.

## 6. display : [[grid]]

- Flex와는 다르게 2차원으로 배치하는 방식으로 column과 row의 비율이나 크기를 지정한다.  

- grid-template-rows는 가로에 배치할 셀들의 비율이나 크기를 지정하는 속성이고 grid-template-columns는 세로로 배치할 셀들의 비율이나 크기를 나타낸다.

- grid-template-rows나 grid-template-columns에 입력할 수 있는 단위는 px, fr이 있는데 fr은 fraction으로 숫자 비율대로 트랙의 크기를 나누는 것이다. 

- 즉 grid-template-columns : 1fr 1fr 1fr 은 1:1:1로 나누는 것이고 grid-template-columns : 100px 3fr 1fr 은 첫번째 셀은 100px, 두번째 셀과 세번째 셀은 남은 부분을 3:1로 나누어 차지한다.

- 다음은 grid-template-rows와 grid-template-columns를 모두 적용한 예시이다.

```html
<style>
    .wrapper {
      display: grid;
      grid-template-rows: 20px 40px;
      grid-template-columns: 30px 35px;
    }

    .item {
      border: 1px solid blue;
    }
  </style>

<body>
  <div class="wrapper">
    <div class="item">1</div>
    <div class="item">2</div>
    <div class="item">3</div>
    <div class="item">4</div>
  </div>
</body>
```

![](https://velog.velcdn.com/images%2Fsukong%2Fpost%2Fd2b79a7c-52f7-44b0-9111-1f5ccfa93b0c%2Fimage.png)

### 7. display : table

- 레이아웃을 table로 표현할 수 있는 속성이다.
	
	- display : table // table 요소처럼 표현하기  
	- display : table-row // tr 요소처럼 표현하기  
	- display : table-column // col 요소처럼 표현하기  
	- display : table-cell // td 요소처럼 표현하기  
	- display : table-caption // caption요소처럼 표현하기