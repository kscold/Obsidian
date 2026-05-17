- background-image 속성은 배경 이미지 경로 및 그라디언트를 설정합니다.

### 웹 문서에 이미지를 표현하는 방법 2가지
- img 태그와 background 속성을 이용하는 방법이다.

- 원칙적으로 의미가 있는 이미지는 img 태그를 사용하고, 그렇지 않은 경우는 background 속성을 사용한다.
- background 속성은 의미가 없는 이미지를 사용하기 위한 방법입니다. 
- 배경이 장식용이거나 디자인 용도라면 background를 이용해서 많이 사용하낟.
- 여기에는 사이즈, 반복여부, 위치, 효과 등 다양한 속성을 사용할 수 있습니다. 
  
기본값:background-image : none|

### 정의(Definition)

- background-image 속성은 배경 이미지 경로 및 그라디언트를 설정한다.
- background-image 속성은 다중 이미지를 익스플로러9 이상부터 지원된다.

### 관련된 속성(Related Properties)

- background 속성은 배경 이미지의 속성을 일괄적으로 설정합니다.
- background-image 속성은 배경 이미지 경로 및 그라디언트를 설정합니다.
- background-position 속성은 배경 이미지의 위치를 설정합니다.
- background-size 속성은 배경 이미지 사이즈를 설정합니다.
- background-repeat 속성은 배경 이미지 반복 여부를 설정합니다.
- background-origin 속성은 배경 이미지의 위치 기준점을 설정합니다.
- background-clip 속성은 배경 이미지의 위치 기준점을 설정합니다.
- background-attachment 속성은 배경 이미지의 고정 여부를 설정합니다.
- background-color 속성은 배경 색을 설정합니다.
- background-blend-mode 속성은 배경 이미지를 혼합했을 때의 상태를 설정한다.

### 문법(Syntax)

background-image : none | url | gradient

```css
/* 기본 속성 */
background-image : none;
background-image : url("../image.jpg");

/* 다중 이미지 설정 */
background-image : url("../image1.jpg"), url("../image2.jpg");

/* 그라디언트 설정 */
background-image : linear-gradient();
background-image : radial-gradient();
background-image : repeating-linear-gradient();
background-image : repeating-radial-gradient();

/* 전역 속성 */
background-image : inherit;   /* 상속 */
background-image : initial;   /* 초기화 */
background-image : revert;    /* 원래대로 돌리기 */
background-image : unset;     /* 설정 해제 */
```

### 속성(Property)

- background-image : none;
백그라운드 이미지를 설정하지 않습니다.|

url
- background-image : url(../img.jpg);|\
백그라운드 이미지 경로를 설정합니다.|

gradient
- background-image : radial-gradient();
백그라운에 그라디언트를 설정합니다.|
