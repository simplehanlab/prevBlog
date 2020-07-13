---

title: "MySQL 사용자 추가, 권한부여"
comments: true
categories:
  - DB
tags:
  - MySQL
date: "2020-07-13 16:47"
---

# MySQL 권한

## MySQL 사용자 추가

사용자 계정을 추가 하기 전에 현재 생성된 사용자 계정을 확인하기 위해서는

```
use mysql;
select host, user from user;
```

쿼리를 통해서 가능합니다.

그 후, 사용자를 추가하고자 한다면

```
create user '사용자 ID';
create user '사용자아이디'@localhost identified by '비밀번호'; //로컬호스트에서만 접속가능
create user '사용자아이디'@'%' identified by '비밀번호'; //외부에서도 접속가능
```

이런식으로 생성이 가능 합니다.



## MySQL 사용자 DB 권한 부여및 제거

MySQL에서 사용자 계정의 권한을 추가하거나 제거할 때에는 GRANT와 REVOKE 명령으로 가능합니다.

현재 사용자에게 허가된 권한을 보기 위해서는

SHOW GRANTS 를 사용합니다.

권한부여를 하면서 비밀번호 변경까지 하려면 IDENTIFIED BY '비밀번호'; 를 통해 가능합니다.

이러한 방식으로 권한을 부여하는 쿼리는 다음과 같습니다.

```
GRANT ALL PRIVILEGES ON DB명.테이블 TO '사용자아이디'@호스트 IDENTIFIED BY '비밀번호'; //모든 권한

GRANT select, insert, update PRIVILEGES ON DB명.테이블 TO '사용자아이디'@호스트 IDENTIFIED BY '비밀번호'; //select, insert, update 권한
```

DB명과 테이블에 *을 주면 모든 DB의 테이블에 전역권한을 부여 할 수 있으나 권장되지는 않습니다.

비밀번호 변경을 원치 않는다면 IDENTIFIED BY '비밀번호'; 를 제거합니다.

이러한 작업을 마친후 반영을 위해

```
flush privileges;
```

명령어를 통해 메모리에 반영하면 됩니다.

반대로 권한을 제거하고자 한다면,

```
revoke all on DB명.테이블명 from '사용자아이디'; 
```

쿼리를 통해 가능합니다.