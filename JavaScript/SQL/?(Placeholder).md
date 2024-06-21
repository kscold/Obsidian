- [[SQL]]에 안에 사용된 ?(물음표)를 지정하여 동적으로 [[쿼리(Query)]]문이 수행될 수 있도록 하는 방법이다.

- SQL에 안에 사용된 ?(물음표)를 Placeholder라고 한다
- 이 Placeholder와  매칭되는 값을 설정해 주는  방식이다.

## 사용법

- 동적 SQL문을 구성하기 위해서는 파라미터값이 들어가는 위치에 Parameter Placeholder를 넣고, execute() [[메서드(Method)]]의 두번째 파라미터에 실제 값들이 저장되어 있는 Tuple 을 넣어 주면 된다.

- Parameter Placeholder 에서는 문자열나 숫자 모두 ?(혹은 Named Placeholder)를 사용하며, 문자열이라도 Placeholder 를 인용부호로 둘러싸지 않는다.
- 즉, 오라클에서 ' ' 를 사용해야하는 값이 있더라도(문자열 등)  '?'가 아닌 ? 만 쓰면 된다.  

- Placeholder는 컬럼값을 대치(매핑)할 때만 사용될 수 있으며 [[테이블(Table)]]이나 기타 문장에는 Placeholder 를 사용할 수 없다.