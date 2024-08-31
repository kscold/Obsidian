- 열거형(Enum은 특정 값(상수)들의 집합을 의미한다. 
- 기본적으로 값은 0부터 1씩 증가하도록 설정된다.

```javascript
enum Color { Red = 0, Green = 1, Blue = 2 }
let c: Color = Color.green
```

또는 모든 값을 수동으로 직접 설정해줄 수도 있다.

```javascript
enum Color { Red = 2, Green = 4, Blue = 8 }
let c: Color = Color.green
```

- enum은 그 자체로 [[객체(Object)]]이기도 하다.
- 그래서 `Object.keys` 를 사용하면 key값이 배열에 담겨 나오기도 한다.  
- 하지만 차이점도 존재한다.

## [[객체(Object)]]와 enum의 차이점

- `object` 는 코드내에서 새로운 속성을 자유롭게 추가가 가능하나 `enum`은 선언 이후 변경할 수 없다.
- `object`의 속성값은 JS의 모든 타입이 올 수 있지만, `enum`은 `string` 혹은 `number` 만 가능하다.