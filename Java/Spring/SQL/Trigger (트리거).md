- 트리거(Trigger)는 사전적 의미로 '방아쇠'라는 뜻이다.

- MySQL에서 트리거는 테이블에서 어떤 이벤트가 발생했을 때 자동으로 실행되는 것을 말한다.

- 즉, 어떤 테이블에서 특정한 이벤트(update, insert, delete)가 발생했을 때, 실행시키고자 하는 추가 쿼리 작업들을 자동으로 수행할 수 있게끔 트리거를 미리 설정해 두는 것이다.

예를 들어 고객이 물건을 구매해 구매 테이블에 정보가 insert되면, 등록된 트리거가 발동해 물품 테이블을 자동으로 update 쿼리문을 실행하게 하고, 또 등록된 트리거가 발동해 배송테이블에 insert 쿼리문을 실행시키게 끔 할 수 있다.

[![mysql-trigger](https://blog.kakaocdn.net/dn/bbErzH/btrjWlEEy5v/Q6tC7FZdUAHHdpBV8Z5Sok/img.png)](https://blog.kakaocdn.net/dn/bbErzH/btrjWlEEy5v/Q6tC7FZdUAHHdpBV8Z5Sok/img.png)

- 데이터베이스 트리거(Database Trigger)는 테이블에 대한 **이벤트에 반응해 자동으로 실행되는 작업**을 의미
- ​테이블에 DML문(Insert, Update, Delete 등) 이벤트가 발생될 때 작동
- 테이블에 부착되는 프로그램 코드
- 자기가 직접 실행 불가. 테이블에 이벤트 일어나야 자동 실행
- IN, OUT 매개 변수를 사용할 수 없음
- MySQL은 View에 트리거 부착 불가

---

### **트리거 종류**

[![mysql-trigger](https://blog.kakaocdn.net/dn/bKAoE8/btrDRcgnStW/YpyBlQkDaPRLDFhKdYOxx0/img.png)](https://blog.kakaocdn.net/dn/bKAoE8/btrDRcgnStW/YpyBlQkDaPRLDFhKdYOxx0/img.png)

#### **행 트리거** 

- 테이블 안의 영향을 받은 **행 각각에 대해 실행**된다.
- **변경 전** 또는 **변경 후**의 행은 **OLD, NEW**라는 가상 줄 변수를 사용하여 읽을 수 있다  
    
    - **old** - 예전 데이터 즉, delete 로 삭제 된 데이터 또는 update 로 바뀌기 전의 데이터  
        
    - **new** - 새 데이터 즉, insert 로 삽입된 데이터 또는 update 로 바뀐 후의 데이터
    

| 트리거 이벤트 | OLD | NEW |
| ------- | --- | --- |
| INSERT  | X   | O   |
| UPDATE  | O   | O   |
| DELETE  | O   | X   |

* update일 경우, 기존의 데이터(old)를 새로운데이터로(new)로 교체하는 거니까 old, new 둘다 쓸 수 있다.  
* 하지만 delete일경우 기존 데이터를 삭제하는 거니 new를 인식할수가 없다.  
* insert역시 새로 삽입하는거니 old가 있을리  없다.


#### **문장 트리거**

- INSERT, UPDATE, DELETE 문에 대해 **한번만 실행**된다
- 삽입, 갱신 또는 삭제되는 행 수에 관계없이 각 트랜잭션에 대해 명령문 레벨 트리거가 한 번 실행된다.

**BEFORE 또는 AFTER** : 트리거가 실행되는 시기를 지정한다.

- after 트리거 : 쿼리 이벤트 작동하기 후
- before 트리거 : 쿼리 이벤트 작동하기 전에 -> 미리 데이터를 확인 가능!

[![mysql-trigger](https://blog.kakaocdn.net/dn/bK25AY/btrDRa3WuO6/YIOFNsx8AUUfRjZvUVZNY0/img.png)](https://blog.kakaocdn.net/dn/bK25AY/btrDRa3WuO6/YIOFNsx8AUUfRjZvUVZNY0/img.png)

**INSTEAD OF** : 트리거를 원래 문장 대신 수행한다.

[![mysql-trigger](https://blog.kakaocdn.net/dn/2kLlX/btrDPCtzLJz/DbEqyrPeQW3KMCAXRZRMgk/img.jpg)](https://blog.kakaocdn.net/dn/2kLlX/btrDPCtzLJz/DbEqyrPeQW3KMCAXRZRMgk/img.jpg)

---

### **트리거 사용법**

#### **트리거 생성**

sql

```
DELIMITER $$

CREATE TRIGGER update_item
AFTER UPDATE  -- {BEFORE | AFTER} {INSERT | UPDATE| DELETE } 중 언제 어떤 작업을 할지 정한다
ON sale_table -- 트리거를 부착할 테이블
FOR EACH ROW -- 아래 나올 조건에 해당하는 모든 row에 적용한다는 뜻

BEGIN
  -- 트리거시 실행되는 코드
  IF NEW.discount_rate != OLD.discount_rate THEN -- update 트리거는 old와 new 값이 존재한다.
    UPDATE item_table SET discount_rate = NEW.discount_rate WHERE discount_rate = OLD.discount_rate;
  END IF;
END $$

DELIMITER ;
```

**BEGIN~END** 사이에 조건문과 실행문을 작성한다.

sale_table 테이블의 변경 전/후를 기준으로 필드 앞에 변경 전은 **OLD**, 변경 후는 **NEW** 키워드가 붙는다

따라서 **IF NEW.discount_rate != OLD.discount_rate THEN**의 의미는,

sale_table의 AFTER UPDATE 후 discount_rate 필드의 값과 변경 전 discount_rate 필드의 값이 불일치한다는 조건을 의미한다.

이 조건을 만족하는 row는 IF문 아래 작성된 **UPDATE 쿼리문**을 실행하게 된다.

**​**

#### **트리거 실행**

TRIGGER update_item 은 **AFTER UPDATE ON sale_table** 이 실행되면 자동으로 트리거가 작동하는 구조라, 당연히 **sale_table에 update쿼리를 주면** 된다.

sql

```
UPDATE sale_table SET ... WHERE ...;
```

#### **트리거 확인**

sql

```
show triggers;
```

[![DBMS/MySQL](https://blog.kakaocdn.net/dn/w30Lx/btrDQd08Gsw/vOzGFH02MGPONVADQ8SGB1/img.png)](https://blog.kakaocdn.net/dn/w30Lx/btrDQd08Gsw/vOzGFH02MGPONVADQ8SGB1/img.png)

**​**

#### **트리거 삭제**

sql

```
DELETE TRIGGERS;
```

---

## **트리거 vs 프로시저**

1. 트리거 이벤트(INSERT | UPDATE | DELETE)가 실행된 테이블을, 트리거를 통해 수정하려 하면 에러 발생  
    
    - 예를 들어 A 테이블에 INSERT 트리거를 생성하는 과정에서 트리거 처리에서 A테이블을 UPDATE를 하게 되는 경우 에러가 발생한다.
    - 트리거 테이블을 수정하려면 이러한 경우에는 프로시저로 처리해야함
    
2. 트리거는 매 이벤트(INSERT | UPDATE | DELETE)마다 동일하게 처리하여 적용하는 경우 사용하고 프로시저는 그렇지 않은 경우 사용한다  
    
    - 예를 들어 통신사가 회원 등급을 결정할 때 전년도 사용 비용을 기준으로 처리하기 때문에 회원 등급을 결정하는 처리를 트리거가 아닌 프로시저로 해야한다.

출처: [https://inpa.tistory.com/entry/MYSQL-📚-트리거](https://inpa.tistory.com/entry/MYSQL-%F0%9F%93%9A-%ED%8A%B8%EB%A6%AC%EA%B1%B0) [Inpa Dev 👨‍💻:티스토리]