- 한쪽 [[테이블(Table)]]에는 하나의 값만 존재하고, 그 값만 대응되는 다른 쪽 테이블의 값은 여러 개인 관계를 말한다.

## 일대다의 예시

![[Pasted image 20240413030438.png]]

- users 모델과 comments 모델 간의 관계를 정의할 때, 일대다 관계에서는 사용자 한명이 댓글 여러 개를 작성한다.

- [[시퀄라이즈(Sequelize)]]에서는 일대다 관계를 hasMany()로 표현한다.

- 사용자.hasMany(댓글) 형식이다.
- 반대 입장에서는 belongTo(댓글.belongsTo(사용자)) 형식이다.
- belongsTo가 있는 [[테이블(Table)]]에 필드([[열(Column)]])가 생긴다.

![[Pasted image 20240413030415.png]]


```sql
CREATE TABLE nodejs.commenets(
	id INT NOT NULL AUTO_INCREMENT,
    commenter INT NOT NULL,
    comment VARCHAR(100) NOT NULL,
    created_at DATETIME NOT NULL DEFAULT now(),
    PRIMARY KEY(id),
    INDEX commenter_idx (commenter ASC),
    CONSTRAINT commenter
    FOREIGN KEY (commenter)
    REFERENCES nodejs.users (id)
    ON DELETE CASCADE
    ON UPDATE CASCADE
)
COMMENT = '댓글'
DEFAULT CHARSET=utf8mb4
ENGINE=InnoDB;
```

- 댓글 [[테이블(Table)]]에 commenter [[열(Column)]]이다.

```sql
CREATE TABLE nodejs.users(
	id INT NOT NULL AUTO_INCREMENT,
    name VARCHAR(20) NOT NULL,
    age INT UNSIGNED NOT NUll,
    married TINYINT NOT NULL,
    comment TEXT NULL,
    created_at DATETIME NOT NULL DEFAULT now(),
    PRIMARY KEY(id),
    UNIQUE INDEX name_UNIQUE (name ASC)
)
COMMENT = '사용자 정보'
DEFAULT CHARACTER SET = utf8
ENGINE = InnoDB;
```

