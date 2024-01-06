- 패키지(package)는 비슷한 성격의 [[클래스(Class)]]들을 모아 놓은 Java의 디렉터리**이다.
- variable라는 패키지를 만들었다면 해당 패키지에 들어가는 자바 파일 첫줄에 package variable;와 같이 소속된 패키지를 선언해주어야 한다.
- 자바 파일이 위치하는 패키지와 package variable 선언 위치가 같아야 한다.

- 패키지를 사용하면 비슷한 성격의 클래스들끼리 묶을 수 있어 클래스의 분류가 용이하다. 
- 그리고 자바 코드를 작성하다 보면 다른 사람이 작성한 자바 클래스나 라이브러리를 사용해야할 경우도 많이 생기는데, 이때 클래스명이 동일한 경우도 발생할 수 있을 것이다.
- 하지만 패키지명이 다르면 클래스명이 동일해도 충돌 없이 사용할 수 있다.

## 서브 패키지

- 서브 패키지는 기본 패키지 안에 존재하는 하위 패키지이다. 
- 이를 사용해 기본 패키지 내의 클래스들을 분류하여 체계적으로 관리하고, 가독성을 향상시킬 수 있다.

- 예를 들어 house 패키지 위에서 마우스 오른쪽 버튼을 클릭해 `[New → Package]`를 선택하여 `house.person`이라는 서브패키지(subpackage)를 만든다고 가정한다.

![](https://wikidocs.net/images/page/231/ij11.png)

- 그다음 person 패키지 위에서 마우스 오른쪽 버튼을 클릭해 `[New → Java Class]`를 선택하여 EungYongPark 클래스를 생성해 보자.

- house/person/EungYongPark.java

```java
package house.person;

public class EungYongPark {
}
```

- EungYongPark 클래스의 package가 `house.person`으로 생성되었다. 이렇게 패키지는 도트(`.`)를 이용하여 서브 패키지를 표시한다. 다시 말해, `house.person`은 house 패키지의 서브패키지이다.

## 패키지 사용하기

- 다른 클래스에서 HouseKim 클래스를 사용하려면 다음과 같이 import를 해야 한다.

```java
import house.HouseKim;

public class Sample {
    public static void main(String[] args) {
        HouseKim kim = new HouseKim();
    }
}
```

- 또는 다음과 같이 `*` 기호를 이용해 house 패키지 내의 모든 클래스를 사용할 수 있다.

```java
import house.*;

public class Sample {
    public static void main(String[] args) {
        HouseKim kim = new HouseKim();
        HousePark park = new HousePark();
    }
}
```

- 만약 HouseKim과 동일한 패키지 내에 있는 클래스(여기서는 HousePark)라면 HouseKim 클래스를 사용하기 위해서 따로 import할 필요는 없다. 같은 패키지 내에서는 import 없이도 사용할 수 있다.

```java
package house;

public class HousePark {
    public static void main(String[] args) {
        HouseKim kim = new HouseKim();  // HouseKim 사용을 위해서 import가 필요없다.
    }
}
```