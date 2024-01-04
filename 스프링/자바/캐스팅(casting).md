- 데이터 타입을 변환하는 것을 말하며 형변환이라고도 한다.
- [[List]]와 [[Set]]은 공통적으로 [[Iterator]]을 사용할 수 있다. 
- 그럼 두 인터페이스가 공통적으로 가지고 있는 이 Iterator라는 속성을 Collection 인터페이스가 가지고 있나?

이러한 궁금증이 생겨 JDK를 하나 하나 까봤다. 

  

정확히는 List, Set => Collection => Iterable의 순서로 implements 하고 있다.

![[Pasted image 20240104211341.png]]