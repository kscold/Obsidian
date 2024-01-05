- [[extends]]로 확장된 클래스를 자식 클래스라고 한다.

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
