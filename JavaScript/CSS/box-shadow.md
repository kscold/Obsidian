- 이 속성은 CSS 요소에 그림자를 추가해준다.


## 문법

```css
box-shadow : offset-x offset-y blur-radius spread-radius color inset;

box-shadow : 5px 5px 5px 5px red inset;
```

### offset-x, offset-y

- 그림자의 위치를 지정합니다.
#### offset-x

- 양수를 입력하면 그림자가 오른쪽으로, 음수를 입력하면 그림자가 왼쪽으로 만들어집니다.
#### offset-y

- 양수를 입력하면 그림자가 아래쪽으로, 음수를 입력하면 그립자가 위쪽으로 만들어집니다.
### blur-radius

-  숫자가 커질수록, 그림자가 블러 처리되어 흐려진다.
### spread-radius

- 숫자가 커질수록, 그림자의 크기가 커진다.
### color

- 그림자의 색깔을 지정할 수 있다.
### inset

- inset이라는 이 키워드를 사용하면, 그림자가 요소의 바깥쪽이 아닌 안쪽에 생성된다.