- LIKE 연산자는 조회 조건 값이 명확하지 않을 때 사용한다.
- LIKE 연산자는 ‘~와 같다’라는 의미이다.

- LIKE 연산자는 %와 _ 같은 [[기호 문자(Wildcard Character)]]와 함께 사용한다.
- 조건에는 문자나 숫자를 포함할 수 있다.

## 예시

- job_id 값에서 AD를 포함하는 모든 정보를 조회한다고 가정한다.

- %는 조건을 포함하는 ‘~ 모든 문자’라는 의미이다.

```sql
SELECT * FROM employees -- employees 테이블에서 모든 필드를 가져옴

WHERE job_id LIKE 'AD%'; -- 맨 앞에 AD라는 문자 값을 가지면서 그 뒤로 모든 문자(%)를 포함하는 레코드를 반환
```

- 즉, employees 테이블에서 job_id 값이 AD를 포함하는 모든(%) 데이터를 조회한다.

![](https://thebook.io/img/006977/079.jpg)