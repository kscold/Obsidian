- 클라이언트가 요청한 정보를 처리하고 다시 응답하기 위한 정보를 담고 있는 클래스이다.

| void setHeader(name, value) | 응답에 포함될 Header를 설정합니다. |
| ---- | ---- |
| void setContentType(type) | 출력되는 페이지의 contentType을 설정 |
| String getCharacterEncoding() | 응답 페이지의 문자 인코딩 Type을 반환 |
| void sendRedirect() | 지정된 URL로 요청을 재전송 |