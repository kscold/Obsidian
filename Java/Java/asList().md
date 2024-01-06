- 일반 배열을 [[ArrayList]]로 변환한다.  

```java
List<String> list = Arrays.asList(arr);
```

- Arrays.asList()는 Arrays의 [[private]] 정적 클래스인 ArrayList를 리턴한다. 
- java.util.ArrayList 클래스와는 다른 클래스이다.
- java.util.Arrays.ArrayList 클래스는 set(), get(), contains() 메서드를 가지고 있지만 원소를 추가하는 메서드는 가지고 있지 않기 때문에 사이즈를 바꿀 수 없다.

|   |
|---|
|package Test;  <br>import java.util.List;  <br>import java.util.ArrayList;  <br>import java.util.Arrays;  <br>   <br>   <br>public class TestArrayAsList {  <br>   public static void main(String[] args) {  <br>      String[] strs = {"alpha", "beta", "charlie"};  <br>      System.out.println(Arrays.toString(strs));   // [alpha, beta, charlie]  <br>   <br>      List<String> lst = Arrays.asList(strs); // new ArrayList<String>(); 대신에 사용  <br>      System.out.println(lst);  // [alpha, beta, charlie]  <br>   <br>      **lst.****add****(****"ttt"****);**     **// error : asList()로 List를 생성하면 원소를 새롭게 추가할 수 없음**  <br>   <br>      // Changes in array or list write thru  <br>      strs[0] += "88";  <br>      **lst.set(****2****, lst.get(****2****)** **+** **"99"****);** **// 2번째 인덱스 원소에 charlie99 넣음**  <br>      **System****.****out****.****println****(Arrays.****toString****(strs));** **// [alpha88, beta, charlie99**]  <br>      System.out.println(lst);  // [alpha88, beta, charlie99]  <br>   <br>      // Initialize a list using an array  <br>      List<Integer> lstInt = Arrays.asList(22, 44, 11, 33);  <br>      System.out.println(lstInt);  // [22, 44, 11, 33]  <br>   }  <br>}  <br>[Colored by Color Scripter](http://colorscripter.com/info#e)|

![](https://mblogthumb-phinf.pstatic.net/MjAxNzExMTVfMTky/MDAxNTEwNjcyMzQyNjMx.erEU2bX1K_Y6OgcPYQdL1a74G72Tye52CQL2k3jHC_og.8mwDn8Tgy60yn4V7KM_X4hWcb4bzrOe4LJfYNUXPP7kg.PNG.roropoly1/image.png?type=w800)

  
위에 소스를 보면 lst에 담겨있는 두 번째 인덱스에 데이터를 수정했는데  
**원본 배열의 데이터까지 변경이 됐다.**  
  
List는 내부 구조가 배열로 만들어져 있다. 따라서 asList()를 사용해서 반환되는 List도  
배열을 갖게 된다.  
  
이때, asList()를 사용해서 List 객체를 만들 때 새로운 배열 객체를 만드는 것이 아니라,  
**원본 배열의 주소값**을 가져오게 된다.  
따라서 asList()를 사용해서 내용을 수정하면 원본 배열도 함께 바뀌게 되고  
원본 배열을 수정하면 그 배열로 만들어뒀던 asList()를 이용한 List 내용도 바뀌게 된다.  
  
이러한 이유 때문에 Arrays.asList()로 만든 List에 **새로운 원소를 추가하거나 삭제 할 수 없다.**  
  
따라서 Arrays.asList()는 배열의 내용을 수정하려고 할 때 List로 바꿔서 편리하게 사용하기 위함.  
  
  
만약 **진짜 ArrayList를 받기 위해서는 다음과 같이 변환하면 된다.**  
ArrayList 생성자는 **java.util.Arrays.ArrayList의 상위(super) 클래스인 Collection Type  
도 받아들일 수 있다.**

|   |
|---|
|List<String> list = new ArrayList<String>(Arrays.asList(arr));|

|   |
|---|
|package Test;  <br>import java.util.List;  <br>import java.util.ArrayList;  <br>import java.util.Arrays;  <br>   <br>   <br>public class TestArrayAsList {  <br>   public static void main(String[] args) {  <br>      String[] strs = {"alpha", "beta", "charlie"};  <br>      System.out.println(Arrays.toString(strs));   // [alpha, beta, charlie]  <br>   <br>      **List****<****String****>** **lst** **=** **new** **ArrayList****<****String****>****(Arrays.asList(strs));**   <br>      System.out.println(lst);  // [alpha, beta, charlie]  <br>   <br>      **lst.****add****(****"ttt"****);**     **// 이제는 에러가 나지 않고 데이터를 추가 시킬 수 있다.**  <br>   <br>      // Changes in array or list write thru  <br>      strs[0] += "88";  <br>      **lst.set(****2****, lst.get(****2****)** **+** **"99"****);** **// 2번째 인덱스 원소에 charlie99 넣음**  <br>      **System****.****out****.****println****(Arrays.****toString****(strs));** **// [alpha88, beta, charlie]**  <br>      **System****.****out****.****println****(lst);**  **// [alpha, beta, charlie99, ttt]**  <br>   <br>      // Initialize a list using an array  <br>      List<Integer> lstInt = Arrays.asList(22, 44, 11, 33);  <br>      System.out.println(lstInt);  // [22, 44, 11, 33]  <br>   }  <br>}  <br>[Colored by Color Scripter](http://colorscripter.com/info#e)|

![](https://mblogthumb-phinf.pstatic.net/MjAxNzExMTVfNDYg/MDAxNTEwNjcyNjM0NDg0.-Q-Pmib7MEgYa1sNQ-PkIYFJ-7-z2-D4l2BcmCA8ScYg.r72XeY_Dk1p32rzM_Ed9Ksqb2cXWP_PTQPfrzZdL2CMg.PNG.roropoly1/2017-11-15_00%3B11%3B21.PNG?type=w800)

 이제는 원본 배열과 lst 객체에 담겨있는 배열 데이터는 별개의 주소값이라고 보면 된다.