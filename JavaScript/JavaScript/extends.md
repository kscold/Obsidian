- 객체 지향 프로그래밍의 특징으로 다형성(Polymorphism)이 있다.
- 즉, 부모 클래스에서 정의한 하나의 매서드를 각 자식 클래스특성에 맞게 다르게 변형해서 사용할 수 있다.

- 먼저 extends를 사용하면 클래스를 확장하여 자식 클래스를 만들 수 있다.


## 예시

```js
class Book {
	constructor(title, author) {
	    this.title = title;
	    this.author = author;
	}
	circulation() {
	    console.log("몇 부나 팔렸을까..");
	}
}

class Novel extends Book {}

let 해리포터 = new Novel("해리포터", "J.K.롤링");

console.log(해리포터.author); // J.K.롤링
해리포터.circulation(); // 몇 부나 팔렸을까..
```

- 자식 클래스에서 동일한 이름으로 메서드를 정의하면 내용을 덮어쓰고(overriding), 더이상 부모의 매서드가 호출되지 않는다.

```js
class Book {
	constructor(title, author) {
	    this.title = title;
	    this.author = author;
	}
	  
	circulation() {
	    console.log("몇 부나 팔렸을까..");
	}
}

class Novel extends Book {
	circulation() {
	    console.log("소설의 판매 부수는..");
	}
}

let 해리포터 = new Novel("해리포터", "J.K.롤링");
해리포터.circulation(); // 소설의 판매 부수는..
```