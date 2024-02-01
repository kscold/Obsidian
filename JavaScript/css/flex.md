- flex를 한마디로 말하자면 flex-container는 flex의 영향을 받는 전체 공간이 되고, 설정된 속성에 따라 각각의 flex-item들이 어떤 형태로 배치되는 것이다.
- flex 속성을 받은 부모와 자식요소는 부모를 flex-container라고 자식을 flex-item이라고 부를 수 있게 된다.  

- 이때, flex-item은 내부 콘텐츠보다 크기가 작아질 수 없다.(내부 콘텐츠만큼의 사이즈가 강제된다.)
- 즉, flex 설정을 명시해 줌으로써 display:flex 일 때, 요소 크기를 명시해줄 수 있다.

- 그래서 속성값들을 flex-item에 선언해야 배치를 할 수 있다.(flex-container에 적용하면 안된다.)

- flex-items에 지정하는 속성으로 [[flex-basis]], [[flex-grow]], [[flex-shrink]] 가진다.

## Flex 특징 및 사용방법

- Flex 레이아웃은 flex-container 안에 있는 flex-item을 중앙정렬이나 균등하게 설정하기 좋다.
- 복잡한 레이아웃도 적은코드로 간결하게 설정이 가능하다.

- 시작은  flex-container(부모 요소)에 display: flex; 를 설정해야한다.

### 방향을 설정하는 방법

- [[flex-direction]]을 방향에 따라 설정해주면된다.  

- row는 ➡️ ➡️ ➡️ , row-reverse는 ⬅️ ⬅️ ⬅️ , column은 ⬇️ ⬇️ ⬇️ , column-reverse는 ⬆️ ⬆️ ⬆️ 이렇게 방향을 설정해줄수있다.

### 정렬하는 방법

- 가로방향 [[justify-content]] 세로방향은 [[align-items]] 이 두개로 정렬할수있다.  

### 아이템이 페이지를 넘어갔을때 설정하는방법

- [[flex-wrap]]을 설정해주면 된다.

### 간단히 코드를 줄이는방법

- [[flex-flow]]를 사용하면 편하다.
