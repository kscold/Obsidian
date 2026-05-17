- [[TypeORM]]의 [[리포지토리(Repository)]] [[메서드(Method)]]로 만약 데이터([[행(Row)]])가 존재하면 지우고 존재하지 않으면 아무런 영향이 없다.
- 즉, 데이터([[행(Row)]])를 삭제할 때 사용된다.

- 대체적으로 [[기본 키(Primary Key)]]를 사용해서 삭제한다.
- 필요하다면 [[FindOptionsWhere]] 조건으로 여러 값을 삭제할 수도 있다.

- [[TypeORM.remove()]]를 이용하면 하나의 아이템을 지울 때 두번 [[데이터베이스(DataBase)]]를 이용해야 하기 때문에 (아이템 유무 + 지우기) [[데이터베이스(DataBase)]]에 한번만 접근해도 되는 delete() [[메서드(Method)]]를 사용한다.

- 삭제 작업이 성공적으로 수행되면 [[DeleteResult]] 객체가 반환되며, 이를 호출한 쪽에서 삭제 결과를 확인할 수 있다.


## 예외 처리

- delete() [[메서드(Method)]]는 삭제를 하지 못해도 200을 보여준다.
- 따라서 삭제를 하지 못했을 경우에는 400대의 [[에러(error)]]를 발생시킬 필요성이 있다.

- 따라서 아래 코드는 [[DeleteResult]]의 affected 속성을 통해 [[NotFoundException()]]를 발생시킨다.

```ts
async deleteBoard(id: number): Promise<void> {  
    const result = await this.boardRepository.delete(id);  
	  
    // 삭제를 못했을 시에는 affected === 0이므로 NotFoundException 404 오류를 반환  
    if (result.affected === 0) {  
        throw new NotFoundException(`Can't find Board with id ${id}`);  
    }  
    console.log('result', result);  
}
```