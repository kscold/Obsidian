
MongoDB에는 ObjectId 타입을 지원하는데 RDBMS에서 Primary Key와 같은 고유한 키를 의미한다.

Collections에 데이터를 넣을때 ObjectId를 직접 넣어주지 않는 이상 자동으로 값이 부여되며 모든 Document 들은 각각의 ObjectId가 생긴다.
만약 ObjectId를 형식에 맞지않게 넣는다거나 중복된 값을 넣게될 경우 예외가 발생된다.


**ObjectId는 3개의 영역으로 나뉘어져 있다**

- 첫번째 4byte는 Unix epoch 이후 초 단위로 측정된 ObjectId의 생성을 나타내는 4바이트 타임스탬프 값
- 두번째 5byte는 프로세스당 한 번 생성 되는 5바이트 임의 값이며 이 임의 값은 기계와 프로세스에 고유
- 세번째 3byte는 임의의 값으로 초기화 되는 3바이트 증분 카운터