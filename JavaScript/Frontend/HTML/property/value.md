- value [[속성(Property)]]은 [[<input>]] 태그의 사용자 초깃값(value)을 명시한다.

- [[Context.Provider]]에서는 value [[props]]를 사용하여 초기값을 정의한다.


## type 속성에 따른 value 속성의 용도

- value [[속성(Property)]]은 [[<input>]] 태그의 type [[속성(Property)]]값에 따라 다른 용도로 사용된다.

### "button", "reset", "submit" 

- 버튼 내의 텍스트를 정의한다.
### "hidden", "password", "text" 

- 입력 필드의 초깃값을 정의한다.
### "checkbox", "image", "radio" 

- 해당 입력 필드를 선택 시 서버로 제출되는 값을 정의한다.
- 또한, [[<input>]] 요소의 type 속성값이 “file”인 경우에는 value [[속성(Property)]]을 사용할 수 없다.