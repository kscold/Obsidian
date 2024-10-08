- [[TypeORM]]에서 삭제 작업을 수행하는 [[메서드(Method)]]들은 DeleteResult 객체를 반환한다. 
- 일반적으로 [[TypeORM.save()]], [[TypeORM.delete()]], [[TypeORM.remove()]] 등의 메서드를 사용하여 데이터를 삭제할 때 DeleteResult를 받을 수 있다.

## raw

- DeleteResult 객체의 raw 속성은 데이터베이스에서 직접 반환한 원시 결과이다.
- 이 속성을 사용하면 데이터베이스가 지원하는 형식으로 결과를 확인할 수 있다. 

- 일반적으로 이 속성을 직접 사용하는 경우는 드물며, 대부분은 affected 속성을 사용하여 삭제된 데이터의 수를 확인한다고 한다.

## affected

- DeleteResult 객체의 affected 속성은 삭제 작업으로 영향 받은 데이터의 수를 나타낸다.  
- 즉, 몇 개의 데이터가 삭제되었는지를 나타낸다.  

- 예를 들어, delete 메서드를 호출하여 데이터를 삭제하면 해당 데이터가 존재하면 1을 반환하고, 존재하지 않으면 0을 반환한다.  
- 만약 삭제 작업이 여러 개의 데이터를 삭제했다면 그에 해당하는 숫자가 반환된다고 한다.
