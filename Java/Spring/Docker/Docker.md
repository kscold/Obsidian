


```
 docker run --name mysql-sample-container -e MYSQL_ROOT_PASSWORD=<password> -d
-p 3306:3306 mysql:{version}
```

- docker를 run 실행하는데 --name 이름은 mysql-sample-container 으로 -e mysql의 이미지  MYSQL_ROOT_PASSWORD의 비밀번호를 설정하고 3306 포트로 mysql:{version} 버전의 이미지로 연결하겠다는 의미이다.

- docker ps -a: 현재 실행중인 도커 컨테이너 목록을 출력하는 명령어이다.
- docker exec -it 도커컨테이너이름 bash
- mysql -u root -p: 도커 마이 sql 접속
## 도커 허브

- 도커에서 제공하는 이미지 허브이다.

## 도커 컴포즈

- 다중 컨테이너를 정의하고 실행하가ㅣ 위한 도구이다.
- YAML 파일을 사용하여 다중 컨테이너를 구성한다.