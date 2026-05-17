- 클라이언트가 세션을 정보를 저장하고 세션 기능을 유지하기 위해서 제공되는 클래스이다.

| String getId() | 해당 세션의 세션 ID를 반환 |
| ---- | ---- |
| long getCreationTime() | 세션의 생성된 시간을 반환 |
| long getLastAccessedTime() | 클라이언트 요청이 마지막으로 시도된 시간을 반환 |
| void setMaxInactiveInterval(time) | 세션을 유지할 시간을 초단위로 설정 |
| int getMaxinactiveInterval() | setMaxInactiveInterval(time)로 지정된 값을 반환. (기본값 30분) |
| boolean isNew() | 클라이언트 세션 ID를 할당하지 않은 경우 true 값을 반환 |
| void invalidate() | 해당 세션을 종료 |