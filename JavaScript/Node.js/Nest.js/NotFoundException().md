-  [[Node.js/Nest.js/NestJS]]에서 서버가 요청된 리소스를 찾을 수 없음([[HTTP 응답 상태(status)]] 404)을 나타낸다.

- 잘못된 경로 또는 요청된 데이터가 존재하지 않을 때 주로 사용된다.

- 예를 들면 없는 예약번호 삭제 등이 있다.


## [[서비스(Service)]]에서 에러처리

- 아래 코드와 같이 if문과 [[throw]]를 통해 오류를 처리한다.
- NotFoundException()에 [[매개변수(parameter)]]로 문자열을 입력하면 오류 메세지도 변경 가능하다.

```ts
// id를 통한 하나의 게시물을 조회  
getBoardById(id: string): Board {  
    const found = this.boards.find((board) => board.id === id); 
	  
    // id를 찾지 못했을 때 오류  
	if (!found) {  
	    throw new NotFoundException(`Can't find Board with id ${id}`);  
	}
	  
    return found;  
}
```

- 아래와 같은 응답이 오게 된다.

```json
{
	"message": "Can't find Board with id 66a01510-6834-11ef-8385-55785b8632ce",
	"error": "Not Found",
	"statusCode": 404
}
```