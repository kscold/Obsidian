- [[BufferedReader]]의 입력이 있으니 출력에도 BufferedWriter [[클래스(Class)]]가 있다.
- System.out.println("") 방식과 같이 출력하는 클래스이고, 이 역시 많은 양의 출력에서 효율적일 수 있다.

```java
//선언
BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
String str = "Print str"; //출력

bw.writer(str); //개행
bw.newLine(); //남아있는 데이터 모두 출력
bw.flush(); //스트림 닫기
bw.close();
```

- 버퍼를 사용하는 클래스이므로 다 쓴 뒤에는 버퍼를 비우고 [[스트림(Stream)]]을 닫는 것이 좋다.