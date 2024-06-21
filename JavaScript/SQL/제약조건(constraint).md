- 제약조건(constraint)이란 데이터의 무결성을 지키기 위해, 데이터를 입력받을 때 실행되는 검사 규칙을 의미한다. 
- 이러한 제약 조건은 [[CREATE]]문으로 [[테이블(Table)]]을 생성할 때나 ALTER 문으로 컬럼을 추가할 때도 설정할 수 있다.  

## 제약 조건의 종류

- MySQL에서 사용할 수 있는 제약 조건은 다음과 같다.  
   
1. NOT NULL
2. UNIQUE
3. PRIMARY KEY
4. FOREIGN KEY
5. DEFAULT

### NOT NULL

- NOT NULL 제약 조건을 설정하면, 해당 필드는 NULL 값을 저장할 수 없다.
- 즉, 이 제약 조건이 설정된 필드는 무조건 데이터를 가지고 있어야 한다.  
   
#### 문법

```sql
CREATE TABLE 테이블이름(    
	필드이름 필드타입 NOT NULL,    
	...
)
```

#### 예시

```sql
CREATE TABLE Test(    
	ID INT NOT NULL,    
	Name VARCHAR(30),    
	ReserveDate DATE,    
	RoomNum INT
);
```


### UNIQUE

- UNIQUE 제약 조건을 설정하면, 해당 필드는 서로 다른 값을 가져야 한다.  
- 즉, 이 제약 조건이 설정된 [[열(Column)]]은 중복된 값을 저장할 수 없다.
   
#### 문법

```sql
CREATE TABLE 테이블이름(   -- 선행 테이블이 존재한다고 가정
	필드명 필드타입 UNIQUE,    
	...
)

CREATE TABLE 테이블이름(    
	필드이름 필드타입,    
	...,
	[CONSTRAINT 제약조건이름] 
	UNIQUE (필드이름)
)
```

- 위의 두 문법은 모두 해당 필드에 UNIQUE 제약 조건을 설정한다.  
- 두 번째 문법을 사용하면, 해당 제약 조건에 이름을 설정할 수 있다.  
   
#### 예시

```sql
CREATE TABLE Test (    
	ID INT UNIQUE,    
	Name VARCHAR(30),    
	ReserveDate DATE,    
	RoomNum INT
);
```


### **PRIMARY KEY**

- PRIMARY KEY 제약 조건을 설정하면, 해당 필드는 NOT NULL과 UNIQUE 제약 조건의 특징을 모두 가진다.
- 따라서 이 제약조건이 설정된 필드는 NULL 값을 가질 수 없으며, 또한 중복된 값을 가질 수 없다.  

- 이러한 PRIMARY KEY 제약 조건을 [[기본 키(Primary Key)]]라고 한다.  

- 테이블의 데이터를 쉽고 빠르게 찾도록 도와주는 역할을 한다.  
   
**문법**

```sql
CREATE TABLE 테이블이름( -- 선행 테이블
	필드이름 필드타입 PRIMARY KEY,    
	...
)

CREATE TABLE 테이블이름(    
	필드이름 필드타입,    
	...,    
	[CONSTRAINT 제약조건이름] 
	PRIMARY KEY (필드이름)
)
```

#### 예시

```sql
CREATE TABLE Test (    
	ID INT PRIMARY KEY,    
	Name VARCHAR(30),    
	ReserveDate DATE,    
	RoomNum INT
);
```


### **FOREIGN KEY** 

- FOREIGN KEY 제약 조건을 설정한 [[열(Column)]]를 [[외래 키(Foreign Key)]]라고 부르며, 한 [[테이블(Table)]]을 다른 [[테이블(Table)]]과 연결해주는 역할을 한다.

- [[외래 키(Foreign Key)]]가 설정된 테이블에 레코드([[행(Row)]])를 입력하면, 기준이 되는 테이블의 내용을 참조해서 레코드([[행(Row)]])가 입력된다.  

- 즉, FOREIGN KEY 제약 조건은 하나의 테이블을 다른 테이블에 의존하게 만든다.  
- FOREIGN KEY를 설정할 때 참조되는 테이블의 필드는 반드시 UNIQUE나 PRIMARY KEY가 설정되어 있어야 한다.
   
#### 문법

```sql
CREATE TABLE 테이블이름(    
	필드이름 필드타입,    
	...,    
	[CONSTRAINT 제약조건이름]   
	FOREIGN KEY (필드이름)    
	REFERENCES 테이블이름 (필드이름))
```

#### 예시

- Test2 테이블의 ParentID 필드에 Test1 테이블의 ID [[열(Column)]]을 참조하는 FOREIGN KEY 제약 조건을 설정하는 예이다.

```sql
CREATE TABLE Test2(    
	ID INT,    
	ParentID INT,    
	FOREIGN KEY (ParentID)    
	REFERENCES Test1(ID) ON UPDATE CASCADE
);
```

#### 1. ON DELETE

- 참조되는 테이블의 값이 삭제될 경우의 동작을 ON DELETE 구문으로 설정한다.
#### 2. ON UPDATE

- 참조되는 테이블의 값이 수정될 경우의 동작을 ON UPDATE 구문으로 설정한다.
   
#### 설정할 수 있는 동작의 종류

- CASCADE와 SET NULL을 가장 많이 사용하고 NO ACTION은 잘 사용하지 않는다.

#### CASCADE 

- 참조되는 테이블에서 데이터를 삭제하거나 수정하면 참조하는 테이블에서도 삭제와 수정이 같이 이루어진다.
#### SET NULL

- 참조되는 테이블에서 데이터를 삭제하거나 수정하면 참조하는 테이블의 데이터는 NULL로 변경된다.
#### NO ACTION

- 참조되는 테이블에서 데이터를 삭제하거나 수정하면 참조하는 테이블의 데이터는 변경되지 않는다. 
#### SET DEFAULT

- 참조되는 테이블에서 데이터를 삭제하거나 수정하면 참조하는 테이블의 데이터는 [[열(Column)]]의 기본값으로 설정한다.
#### RESTRICT

- 참조하는 테이블에 데이터가 남아 있으면 참조되는 테이블의 **데이터를** 삭제하거나 수정할 수 없다.
 
### DEFAULT

- DEFAULT 제약 조건은 해당 필드의 기본값을 설정할 수 있게 해준다.  
- 만약 레코드([[행(Row)]])를 입력할 때 해당 필드([[열(Column)]]) 값을 전달하지 않으면, 자동으로 설정된 기본 값을 저장한다. 

- 만약 이모티콘 값은 문자를 설정하고 싶다면 DEFAULT CHARSET=utf8mb4로 절정하면 된다.
#### 문법

```sql
CREATE TABLE 테이블이름(    
	필드이름 필드타입 DEFAULT 기본값,    
	...
)
```

#### 예시

```sql
CREATE TABLE Test(    
	ID INT,    
	Name VARCHAR(30) DEFAULT 'Anonymous',    
	ReserveDate DATE,    
	RoomNum INT
);
```

