- 웹 서비스에 사용되는 HTTP(Hyper Tranfer Protocol) 프로토콜(protocol)이다.
- HTTP는 클라이언트의 다양한 요청을, [[메서드(Method)]]를 통해 서버로 보내는 역할을 한다.

## HTTP의 종류

- 데이터의 생성, 조회, 수정, 삭제는 데이터 관리에 가장 기본이 되는 동작이기 때문에 [[CRUD]]라고도 한다.

- POST: 데이터 생성 요청이다.
- GET: 데이터 조회 요청이다.
- PATCH(PUT): 데이터 수정 요청이다.
- DELETE: 데이터 삭제 요청이다.

## HTTP Status

### 2xx

- 성공을 알리는 상태코드 200(성공)이다.
### 3xx

- 다른 페이지로 이동이다.
### 4xx

- 요청 오류, 401(권한 없음, 404(찾을 수 없음)이다.
### 5xx

- 서버오류이다.

## HTTP 메세지의 구성
- HTTP 메세지는 시작라인, 헤더, 빈 라인, 본문으로 구성된다.

	- ### 시작라인(start line)
		- HTTP 요청 또는 응답 내용이 있다.  
		- 시작 라인은 항상 한줄로 끝난다.
	- ### 헤더(header)
		- HTTP 전송에 필요한 부가 정보(metadata)가 있다.
	- ### 빈라인(blank line)
		- 헤더의 끝을 알리는 빈 줄로, 헤더가 모두 전송되었음을 알린다.
	- ### 본문(body)
		- 실제 전송하는 데이터가 있다.


## HTTP 요청(Request)

### 시작 라인(Method)

- HTTP 요청은 서버가 특정 동작을 취하게끔 만들기 위해 클라이언트에서 전송하는 메시지이다.
- 시작 라인은 다음과 같이 세 가지 요소로 이루어져 있다.

- HTTP [[메서드(Method)]] 로, 영어 동사(GET, POST, PUT, PATCH, DELETE)를 사용해 서버가 수행해야 할 동작을 나타낸다.
-  예를 들어, `GET`은 리소스를 클라이언트로 가져다 달라는 것을 뜻하며, `POST`는 데이터가 서버로 들어가야 함을 의미(리소스를 새로 만들거나 수정하기 위해, 또는 클라이언트로 돌려 보낼 임시 문서를 생성하기 위함이다.)한다.

- '요청 타겟'은 주로 URL, 또는 프로토콜, 포트, 도메인의 절대 경로로 나타낼 수도 있으며 이들은 요청 컨텍스트에 의해 특정지어 진다.
- 요청 타겟 포맷은 HTTP 메서드에 따라 달라진다.
- 포맷에는 다음과 같은 것들이 있다.
    - 가장 일반적인 형식이고 'origin 형식'으로 알려진 절대 경로이다.
    - 이 형식은 끝에 `'?'`와 쿼리 문자열이 따라온다. 
    
    - `GET`, `POST`, `HEAD`, `OPTIONS` 메서드와 함께 사용한다.
        - `POST / HTTP/1.1`
        - `GET /background.png HTTP/1.0`
        - `HEAD /test.html?query=alibaba HTTP/1.1`
        - `OPTIONS /anypage.html HTTP/1.0`
    - 'absolute 형식'으로 알려진 완전한 URL은 프록시에 연결하는 경우 대부분 `GET`과 함께 사용된다. `GET http://developer.mozilla.org/ko/docs/Web/HTTP/Messages HTTP/1.1`
    
    - 'authority 형식'으로 알려지고 도메인 이름 및 옵션 포트(`':'`가 앞에 붙는다)로 이루어진 URL의 인증 컴포넌트 입니다. HTTP 터널을 구축하는 경우에만 `CONNECT`와 함께 사용할 수 있다. `CONNECT developer.mozilla.org:80 HTTP/1.1`
    
    - `OPTIONS`와 함께 별표(`'*'`) 하나로 서버 전체를 나타내는 'asterisk 형식'이다. `OPTIONS * HTTP/1.1`

-  메시지의 남은 구조를 정의하는 HTTP 버전은 응답 메시지에서 써야 할 HTTP 버전을 알려주는 역할을 합니다.

### 헤더(Header)

- 요청에 들어가는 HTTP 헤더는 HTTP 헤더의 기본 구조를 따른다. 
- 대소문자 구분없는 문자열 다음에 콜론(`':'`)이 붙으며, 그 뒤에 오는 값은 헤더에 따라 달라집니다. 헤더는 값까지 포함해 한 줄로 구성되지만, 꽤 길어질 수 있습니다.

- 다양한 종류의 요청 헤더가 있는데, 이들은 다음과 같이 몇몇 그룹으로 나눌 수 있다.

- Via와 같은 General 헤더는 메시지 전체에 적용된다.
- `User-Agent` (en-US) `Accept`와 같은 Request 헤더는 요청의 내용을 좀 더 구체화 시키고(`Accept-Language`), 컨텍스를 제공하기도 하며(`Referer`), 조건에 따른 제약 사항을 주기도 하면서(`If-None` "This is a link to an unwritten page") 요청 내용을 수정한다.

- 메시지 데이터의 원래 형식과 적용된 인코딩을 설명하는 `Content-Type`과 같은 Representation 헤더이다.(메시지에 본문이 있는 경우에만 존재한다.)

![HTTP 요청의 헤더 예시](https://developer.mozilla.org/ko/docs/Web/HTTP/Messages/http_request_headers3.png)

### 본문(body)

- 본문은 요청의 마지막 부분에 들어간다.
- 모든 요청에 본문이 들어가지는 않는다.
- `GET`, `HEAD`, `DELETE` , `OPTIONS`처럼 리소스를 가져오는 요청은 보통 본문이 필요가 없다.
- 일부 요청은 업데이트를 하기 위해 서버에 데이터를 전송한다.
- 보통 (HTML 폼([[<form>]]) 데이터를 포함하는) `POST` 요청일 경우에 그렇다.

- 넓게 보면 본문은 두가지 종류로 나뉜다.
	- 헤더 두 개(`Content-Type`와 `Content-Length`)로 정의된 단일 파일로 구성되는 단일-리소스 본문(single-resource bodies)이다.
	- 각각 서로 다른 정보를 담고 있는 멀티파트 본문으로 구성되는 다중 리소스 본문이다. 
	- 보통 HTML 폼 "Currently only available in English (US)")과 관련이 있다.

## HTTP 응답(Response)

### 상태 라인(status line)

- HTTP 응답의 시작 줄은 '상태 라인(status line)'' 이라고 불리며, 다음과 같은 정보를 가지고 있다.

1. 보통 `HTTP/1.1`인 프로토콜 버전이다.
2. 요청의 성공 여부를 나타내는 '상태 코드'이며, 일반적인 상태 코드는 200, 404 혹은 302이다.
3. 사람이 HTTP 메시지를 이해할 때 도움이 되는 상태 코드에 대한 짧고, 순전히 정보 제공 목적의 '상태 텍스트'이다.

- 상태 줄은 일반적으로 `HTTP/1.1 404 Not Found.` 같이 표현된다.

### 헤더(Header)

- 응답에 들어가는 HTTP 헤더는 다른 헤더와 동일한 구조를 따른다.
- 대소문자를 구분하지 않는 문자열 다음에 콜론(`':'`)이 오며, 그 뒤에 오는 값은 구조가 헤더에 따라 달라진다.
- 헤더는 값을 포함해 전체를 한 줄로 표시한다.

- 다양한 종류의 응답 헤더가 있는데, 이들은 다음과 같이 몇몇 그룹으로 나눌 수 있다.

- `Via`와 같은 General 헤더)는 메시지 전체에 적용된다.
- `Vary`와 `Accept-Ranges`와 같은 Response 헤더는 상태 줄에 포함되지 않은 서버에 대한 추가 정보를 제공한다.
- 메시지 데이터의 원래 형식과 적용된 인코딩을 설명하는 `Content-Type`와 같은 Representation 헤더이다.(메시지에 본문이 있는 경우에만 존재한다.)

![HTTP 응답의 헤더 예제](https://developer.mozilla.org/ko/docs/Web/HTTP/Messages/http_response_headers3.png)

### 본문(body)

- 본문은 응답의 마지막 부분에 들어간다.
- 모든 응답에 본문이 들어가지는 않는다.
- 해당 페이로드 없이도 요청에 충분히 응답하는 `201 Created`, `204 No Content`과 같은 상태 코드를 가진 응답에는 보통 본문이 없다.
- 404 같은 경우에는 본문은 ({}) 비어있다.

- 넓게 보면 본문은 세가지 종류로 나뉜다.
	- `Content-Type`와 `Content-Length`라는 두 개의 헤더로 정의하는 길이가 알려진 하나의 파일로 구성된 단일-리소스 본문(Single-resource bodies)이다.
	- `Transfer-Encoding`가 `chunked`로 설정된 청크로 나뉘어 인코딩되는 길이를 모르는 하나의 파일로 구성된 단일-리소스 본문이다.
	- 서로 다른 정보를 담고 있는 멀티파트 본문으로 이루어진 다중 리소스 본문인 경우, 상대적으로 위의 두 경우에 비해 보기 힘들다.