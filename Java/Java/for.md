- 확장 for문(향상된 for문) JDK1.5 이상확장 for문은 JDK 1.5이상 부터 배열과 컬렉션에 저장된 요소에 기존 for문 보다 접근하기 편리한 방법으로 처리할 수 있도록 새롭게 추가되었다.
- 확장 for문 구조확장 for문의 구조는 위와 같으며, 세미콜론(;)이 아닌 콜론(:)으로 구분한다. 


## 문법

- 변수명 : [[배열(Array)]]명으로 선언한 뒤, 변수명을 출력하면 해당 인덱스대로 [[배열(Array)]]원소값이 출력된다. 
- 확장 for문은 일반적인 for문과 달리 [[배열(Array)]]과 [[컬렉션(Collection)]]에 저장된 요소들을 읽어오는 용도로만 사용할 수 있다. 

```java
for (자료형 변수: 배열 혹은 컬렉션) {
	System.out.print(변수); // 반복되어 출력
}
```

- 확장 for문 예제 1위의 예제를 보면, k변수를 선언하고, score 배열에 대한 원소값을 출력하는 확장 for문이다. 
- 또는 sum변수를 선언하고, sum+=k; 문장을 추가해 score변수의 원소값의 누적합들도 구할 수 있다.

## 예시

```java
String rev = " ";

// 향상된 확장 for반복문으로 출력
for(String str:names) {
	rev = str + rev;
	System.out.print(str + " ");
}

System.out.print("\n" + rev);
```