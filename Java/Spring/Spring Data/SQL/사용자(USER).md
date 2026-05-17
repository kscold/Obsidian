## 사용자 목록 보기

- mysql 데이터베이스의 user 테이블에서 필요한 정보 출력

SELECT User, Host FROM mysql.user;

## 사용자 만들기

- 사용자 이름 jb, 내부에서만 접속 가능, 비밀번호 1234

CREATE USER 'jb'@'localhost' IDENTIFIED BY '1234';

- 사용자 이름 jb, 어디에서나 접속 가능, 비밀번호 1234

CREATE USER 'jb'@'%' IDENTIFIED BY '1234';

- 사용자 이름 jb, 192.168.0.111에서 접속 가능, 비밀번호 1234

CREATE USER 'jb'@'192.168.0.111' IDENTIFIED BY '1234';

- 이미 존재하는 사용자 이름으로 사용자를 만들려고 하면 에러가 난다. OR REPLACE를 붙이면, 이미 존재하는 사용자 이름인 경우 기존 사용자를 삭제하고 다시 만든다.

CREATE OR REPLACE USER 'jb'@'localhost' IDENTIFIED BY '1234';

- IF NOT EXISTS를 붙이면, 같은 이름의 사용자가 없을 때만 사용자를 추가한다.

CREATE USER IF NOT EXISTS 'jb'@'localhost' IDENTIFIED BY '1234';

## 사용자 이름 변경하기

- 'jb'@'localhost'를 'hs'@'%'로 변경

RENAME USER 'jb'@'localhost' TO 'hs'@'%';

## 사용자 비밀번호 변경하기

- 'jb'@'localhost'의 비밀번호를 12345로 변경

SET PASSWORD FOR 'jb'@'localhost' = PASSWORD( '12345' );

## 사용자 삭제하기

- 'jb'@'%' 삭제

DROP USER 'jb'@'%';

- 'jb'@'%'가 존재하는 경우에 삭제

DROP USER IF EXISTS 'jb'@'%';

## 권한 부여하기

- jb@localhost에게 test 데이터베이스의 모든 테이블에 대한 모든 권한 부여

GRANT ALL PRIVILEGES ON test.* TO 'jb'@'localhost';

- jb@localhost가 가진 권한 출력

SHOW GRANTS FOR 'jb'@'localhost';

## 권한 제거하기

jb@localhost가 test 데이터베이스에 가진 모든 권한을 제거

REVOKE ALL on test.* FROM 'jb'@'localhost';