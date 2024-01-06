- 일반 [[배열(Array)]]을 [[ArrayList]]로 변환한다. 
- Arrays.asList는 리스트를 초기화할 때 자주 사용된다.
- 처음에 다 초기화를 해버리는 Array와 달리 List는 빈 리스트를 만든 후 add를 해주는 식으로만 초기화를 해줄 수 있다는 점이 매우 불편하기 때문이다.
- 이 Arrays.asList를 사용할 때에는 주의할 점이 있다.
- 이 메서드를 사용할 경우, 이를 할당받는 [[변수(Variable)]]는 원래 만들어진 [[배열(Array)]]의 [[인스턴스(Instance)]]를 가리킨다.
- 이런 이유 때문에 위의 방법으로 초기화된 리스트는 ArrayList의 특성(변경이 자유로운)을 갖지 못한다.(따라서 add() 메서드를 사용하지 못한다.)

```java
List<String> list = Arrays.asList(arr);
```

- Arrays.asList()는 Arrays의 [[private]] 정적 클래스인 ArrayList를 리턴한다. 

- java.util.ArrayList [[클래스(Class)]]와는 다른 클래스이다.
- java.util.Arrays.ArrayList 클래스는 remove(), add() 메소드를 제공하지 않고 set(), get(), contains() [[메서드(Method)]]를 가지고 있지만 원소를 추가하는 메서드는 가지고 있지 않기 때문에 사이즈를 바꿀 수 없다.

```java
import java.util.List;  
import java.util.ArrayList;  
import java.util.Arrays; 

public class TestArrayAsList {  
	public static void main(String[] args) {  
		String[] strs = {"alpha", "beta", "charlie"}; // 배열 선언
		System.out.println(Arrays.toString(strs));   // [alpha, beta, charlie] 
		List<String> lst = Arrays.asList(strs); // new ArrayList<String>(); 대신에 사용
		  
		System.out.println(lst); // [alpha, beta, charlie] 
		lst.add("ttt"); // error : asList()로 List를 생성하면 원소를 새롭게 추가할 수 없음
		
		strs[0] += "88";  
		lst.set(2, lst.get(2) + "99");  
		// 2번째 인덱스 원소를 가져와 99를 덧붙이고 2번째 인덱스에 완성된 charlie99 넣음
		
		System.out.println(Arrays.toString(strs)); // [alpha88, beta, charlie99]  
		System.out.println(lst);  // [alpha88, beta, charlie99]
		// Initialize a list using an array 
		List<Integer> lstInt = Arrays.asList(22, 44, 11, 33); 
		System.out.println(lstInt);  // [22, 44, 11, 33] 
	}
}
```

- 위에 소스를 보면 lst에 담겨있는 두 번째 인덱스에 데이터를 수정했는데 원본 배열의 데이터까지 변경이 됐다.

- [[List]]는 내부 구조가 [[배열(Array)]]로 만들어져 있다. 
- 따라서 asList()를 사용해서 반환되는 [[List]]도 [[배열(Array)]]을 갖게 된다.  
  
- 이때, asList()를 사용해서 List [[객체(Object)]]를 만들 때 새로운 배열 객체를 만드는 것이 아니라, 원본 배열의 주소값을 가져오게 된다. 
- 따라서 asList()를 사용해서 내용을 수정하면 원본 배열도 함께 바뀌게 되고 원본 배열을 수정하면 그 배열로 만들어뒀던 asList()를 이용한 List 내용도 바뀌게 된다.

- 이러한 이유 때문에 Arrays.asList()로 만든 List에 새로운 원소를 추가하거나 삭제 할 수 없다.([[add()]], remove() 불가능)
- 따라서 Arrays.asList()는 배열의 내용을 수정하려고 할 때 List로 바꿔서 편리하게 사용하기 위함이다.
  
  
- 만약 진짜 ArrayList를 받기 위해서는 다음과 같이 변환([[캐스팅(casting)]])하면 된다.  
- ArrayList 생성자는 java.util.Arrays.ArrayList의 [[부모 클래스(super class)]](상위(super) 클래스)인 [[Collection]] Type도 받아들일 수 있다.

```java
List<String> list = new ArrayList<String>(Arrays.asList(arr)); // List로 업캐스팅
```

```java
package Test;  

import java.util.List;  
import java.util.ArrayList;  
import java.util.Arrays;  
   
public class TestArrayAsList {  
   public static void main(String[] args) {  
      String[] strs = {"alpha", "beta", "charlie"};  
      System.out.println(Arrays.toString(strs));   // [alpha, beta, charlie]  
   
      List<String>lst = new ArrayList<String>(Arrays.asList(strs)); 
      System.out.println(lst);  // [alpha, beta, charlie]  
   
      lst.add()"ttt");// 이제는 에러가 나지 않고 데이터를 추가 시킬 수 있다.
   
      // Changes in array or list write thru  
      strs[0] += "88";  
      lst.set(2, lst.get(2) + "99"); // 2번째 인덱스 원소에 charlie99 넣음  
      System.out.println(Arrays.toString(strs)); // [alpha88, beta, charlie]
      System.out.println(lst); // [alpha, beta, charlie99, ttt]**  
   
      // Initialize a list using an array  
      List<Integer> lstInt = Arrays.asList(22, 44, 11, 33);  
      System.out.println(lstInt);  // [22, 44, 11, 33]  
   }  
}  
```

![](https://mblogthumb-phinf.pstatic.net/MjAxNzExMTVfNDYg/MDAxNTEwNjcyNjM0NDg0.-Q-Pmib7MEgYa1sNQ-PkIYFJ-7-z2-D4l2BcmCA8ScYg.r72XeY_Dk1p32rzM_Ed9Ksqb2cXWP_PTQPfrzZdL2CMg.PNG.roropoly1/2017-11-15_00%3B11%3B21.PNG?type=w800)

 - 위에 예시 코드를 실행해 보면 이제는 원본 배열과 lst 객체에 담겨있는 배열 데이터는 별개의 주소값이라고 볼 수 있다.