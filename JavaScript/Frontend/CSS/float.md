- float 프로퍼티는 해당 요소를 다음 요소 위에 떠 있게 한다.

- 여기서 떠 있다(float)는 의미는 요소가 기본 레이아웃 흐름에서 벗어나 요소의 모서리가 페이지의 왼쪽이나 오른쪽에 이동하는 것이다.
- 그래서 보통 레이아웃을 구성할 때 요소를 가로 정렬하기 위해 사용되는 기법이다.

- 예를 들어, 문서에 사진과 그림이 있을 때, 그림을 왼쪽이나 오른쪽으로 띄워서 정렬 하거나 각 객체를 오른쪽이나 왼쪽으로 정렬하여 전체 문서를 배치(layout)할 때도 사용할 수 있다.

[![Style Sheet/CSS](https://blog.kakaocdn.net/dn/baHmjO/btr63moSCe2/Bzi7JfrtXikXz0MH0oqpA0/img.png)]

## 속성

### none

- 요소를 떠 있게 하지 않는다. 
- 기본값이다.
### right

- 요소를 오른쪽으로 이동시킨다.
### left

- 요소를 왼쪽으로 이동시킨다.

[![Style Sheet/CSS](https://blog.kakaocdn.net/dn/c2CWfe/btr65CY4d1p/HVuoAvKIcX9YVvhBEYcPAk/img.png)]

## 문법

- 부유하게 하고 싶은 요소에 float: left 로퍼티를 사용하면 왼쪽부터 가로 정렬되고, float: right 프로퍼티를 사용하면 오른쪽부터 가로 정렬된다.

```css
img {
    float: left;
}
```


## float 취소

### clear 속성

- CSS의 clear 속성은 별도의 속성으로서, 주요 기능은 float 속성이 적용된 요소 다음에 위치하는 요소들이 float 속성이 적용된 요소와 겹치는 현상을 방지하기 위해 사용된다.

- 요소에 float 속성이 적용되면 그 이후에 등장하는 모든 요소들은 정확한 위치를 설정하기가 매우 힘들어 지게 되는데, 따라서 clear 속성을 사용하여, 이후의 요소들이 더는 float 속성에 영향을 받지 않도록 설정해주어 요소들이 float 속성이 적용된 요소 아래에 위치하도록 한다.

- clear 속성에도 float 속성과 같이 left, right, both, none 값이 있다.


### none

- clear를 설정하지 않은 기본값이다.
- 요소가 어느 쪽으로든 부유할 수 있다.
### right

- float: right 를 취소시킨다.
- 요소가 왼쪽에 부유한 요소 다음에 위치하도록 한다.
### left

- float :left 를 취소시킨다.
- 요소가 오른쪽에 부유한 요소 다음에 위치하도록 한다.
### both

- float:left , float:right 둘다 취소시킨다.
- 요소가 왼쪽과 오른쪽에 부유한 요소 다음에 위치하도록 한다.

- 예를 들어 아래 예제와 같이 이미지에 float: right 속성을 주게되면 이미지는 오른쪽에 배치되고 그 다음 문단 요소는 자동으로 왼쪽에 배치되게 된다.

```html
<div>
	<img src="https://taegon.kim/wp-content/uploads/2018/05/image-5.png">
</div>
<p>
	Lorem Ipsum is simply dummy text of ...
</p>
```

```css
img {
	float: right;
	width: 25%;
}
```

[![float-clear](https://blog.kakaocdn.net/dn/YmFBe/btr60gpAPfi/Gk80BySfzuxhRVqfyImWqk/img.png)](https://blog.kakaocdn.net/dn/YmFBe/btr60gpAPfi/Gk80BySfzuxhRVqfyImWqk/img.png)

- 만일 문단 영역을 이미지 아래에 위치하도록 하고 싶다면 float: right 를 취소시키는 clear: right를 지정 해주면 된다.


```css
img {
	float: right;
	width: 25%;
}

p {
	clear: right; // p 태그는 float속성에 영향을 받지 않음
}
```

[![float-clear](https://blog.kakaocdn.net/dn/ENz90/btr60fxt8JC/s2rUKMkyLLc6x4Kq8ZUAgk/img.png)](https://blog.kakaocdn.net/dn/ENz90/btr60fxt8JC/s2rUKMkyLLc6x4Kq8ZUAgk/img.png)


## **float 사용시 주의사항**

- float 프로퍼티를 사용할 때 요소의 위치를 고정시키는 [[position]] 프로퍼티의 absolute를 사용하면 안된다.
- float 속성이 relative한 위치 지정을 하기 때문에 position: absolute속성이 적용되지 않기 때문이다.

- 또한 float 속성을 사용하면 해당 요소는 일반적인 흐름에서 벗어나게 되어 요소의 부모 요소는 해당 요소의 높이를 인식하지 못하게 되는데, 이 경우 부모 요소에 overflow: hidden속성을 추가하여 해결할 수 있다.