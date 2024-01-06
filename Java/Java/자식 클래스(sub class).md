- [[extends]]로 상속 받은 [[클래스(Class)]]를 자식 클래스(서브 클래스)라고 한다.

```java
// 부모 클래스(슈퍼 클래스)
class Animal {
    String name;

    public Animal(String name) {
        this.name = name;
    }

    public void eat() {
        System.out.println(name + " is eating.");
    }
}

// 자식 클래스(서브 클래스)
class Dog extends Animal {
    public Dog(String name) {
        super(name); // 부모 클래스의 생성자 호출
    }

    public void bark() {
        System.out.println(name + " is barking.");
    }
}
```

- 자식 클래스에서 부모 클래스의 생성자를 호출할 때는 [[super()]] 키워드를 사용한다.
