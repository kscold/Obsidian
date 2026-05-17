- 최상위 값은 `<html>` 태그를 가진다.

## attribute

- Attributes는 HTML 요소의 추가적인 정보를 전달하고 이름 = “값” 이렇게 쌍으로 온다.
- 예를 들어 `<div class=“my-class”></div>` 를 보면 [[<div>]] 태그가 class 라는 값이 ‘my-class’인 attribute를 가지고 있다.

## property

- Property는 attribute에 대한 HTML [[DOM(Document Object Model)]] 트리안에서의 표현이다.
- 그래서 위 예시에서 attribute는 값이 ‘my-class’이며 이름이 ‘className’인 property를 가진다.

```
Our DIV node  
|- nodename = "DIV"  
|- className = "my-class"  
|- style  
    |- . . .  
|- . . .
```

## attribute 와 property의 차이

- Attributes는 HTML 텍스트 문서에 있는 것이고 properties는 HTML [[DOM(Document Object Model)]] 트리에 있는 것이다. 
- 이것은 attribute는 변하지 않고 초기 (default)값을 전달한다는 것은 의미한다. 

- 반면에 HTML properties는 변할 수 있다. 
- 예를 들어 사용자가 체크박스를 체크했거나 input 박스에 텍스트를 넣었거나 자바스크립트로 값을 변경하면 property의 값은 변한다.

![](https://miro.medium.com/v2/resize:fit:500/0*MM0sob6OFnfbn5ID.png)

- 위의 이미지를 설명하면 사용자가 그의 이름 "Joe"를 input박스에 입력했다고 가정하자. 
- 그러면 attribute와 property값이 아래 이미지와 같이 된다.

![](https://miro.medium.com/v2/resize:fit:500/0*6JPMZPsSEFjKJ0Zb.png)

- 보시다시피, property의 값만 변경되었다. 
- 왜냐하면 property는 [[DOM(Document Object Model)]]안에 존재하고 동적이기 때문이다.
- 그러나 attribute는 HTML 문서안에 존재하고 결코 변하지 않는다.

