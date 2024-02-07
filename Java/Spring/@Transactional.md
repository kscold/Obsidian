- 보통 [[트랜젝션(transaction)]]은 서비스에서 관리한다.
- [[서비스(Service)]]의 메서드에 @Transactional [[어노테이션(Annotation)]]을 붙이면 해당 [[메서드(Method)]]는 하나의 트랜잭션으로 묶인다.
- 이 [[어노테이션(Annotation)]]을 선언하면 [[클래스(Class)]]의 모든 [[메서드(Method)]]별로 각각의 [[트랜젝션(transaction)]]이 부여된다.

- 따라서 api 호출에 실패한 경우, 추가된 데이터 없이 중간에 롤백 된다.