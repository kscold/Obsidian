- [[상대단위]] 중에 글꼴을 나타내는 단위로 사용되고 있는 요소의 font-size를 기준으로 px로 바뀌어 화면에 표시된다. 
- 같은 엘리먼트에 설정된 폰트 크기 값이 없을 경우에는 상위 요소의 폰트 사이즈가 기준이 된다.

- em과 rem에서 차이가 발생하는 것은 이 1 rem의 기준을 어느 것으로 정하느냐 따라 달라진다.(보통 브라우저의 기본 폰트 크기는 16px이다.)

## em
- em은 요소의 글꼴 크기를 1em으로 갖는다.
- 만약 해당 요소의 폰트 크기가 없으면 부모 요소의 글꼴 크기를 1em으로 갖는다

```html
<style>
  .container {
    font-size: 14px; /* 기본 1em */
  }

  .container.title {
    font-size: 2rem; /* 28px */
  }

  .container.subtitle {
    font-size: 1.5rem; /* 21px */
  }

  .container.subtitle.leading {
    font-size: 0.5rem; /* 10.5px */
  }
</style>  

<body>
  <div class="container">
    <div class="title"></div>
    <div class="subtitle"></div>
  </div>  
</body>
```

- 예를 들어 위와 같이 코드가 있다고 가정했을 때, title과 subtitle이 있는 요소에서 font-size를 규정하지 않았기 때문에 title과 subtitle은 윗 상단의 container의 font-size를 1em으로 갖게 된다.
- 그래서 title의 글꼴 크기는 `14px * 2 = 28px`, subtitle의 글꼴 크기는 `14px * 1.5 = 21px`, leading의 글꼴 크기는 `14 * 1.5 * 0.5 = 10.5px`이 된다.

  - 여기서부터 rem과의 차이가 발생하게 된다.
  - 코드를 적다 보면 중첩이라는 게 발생할 수밖에 없다. 
  - 중첩이란 여러 요소들이 서로를 포함하는 것을 말하는데 [[HTML]]을 작성할 때 당연히 일어날 수밖에 없다.

## rem
- em이 해당 단위가 사용되고 있는 요소의 font-size가 기준이 되었다면 rem은 r = root 최상위 요소를 font-size 1 rem으로 여기고 변화한다.
- [[HTML]]에서 최상위 값은 `<html>`으로 rem의 경우는 html요소의 font-size를 기준으로 한다.
- 따라서 중첩이 일어나더라도 em과 달리 기준이 되는 font-size가 변하지 않는다.

- html의 폰트 사이즈가 10px이라면 해당 html문서 안에 있는 모든 요소의 1 rem = 10px이 된다.

```html
<style>
  .container {
    font-size: 14px; /* 기본 1rem */
  }

  .container.title {
    font-size: 2rem; /* 28px */
  }

  .container.subtitle {
    font-size: 1.5rem; /* 21px */
  }

  .container.subtitle.leading {
    font-size: 0.5rem; /* 7px */
  }
</style>  

<body>
  <div class="container">
    <div class="title"></div>
    <div class="subtitle"></div>
  </div>  
</body>
```

- rem에서 leading의 글꼴 크기는 `14 * 0.5 = 7px`이 된다.

- 실제로 많은 CSS 가이드들이 em을 사용해야 하는 타당한 이유가 없다면 rem을 우선적으로 사용하기를 권고하고 있다.
- 왜냐면 em을 사용하게 될 때 영향을 받게 되는 변수 px들이 많아지기 때문에 추후 관리에 있어 어려움이 많기 때문이다.

- 만약 rem과 em을 혼용하여 사용하게 된다면 페이지 어디에 사용해도 사이즈가 고정되어야 하는 곳에 rem, 부모 요소에 따라 유동적으로 변화해야 하는 곳에 em을 사용하는 것이 좋다. 
- 대부분 font-size는 rem을 padding이나 margin 등 화면에 따라 유연하게 변하는 크기는 em을 많이 사용한다.