---

title: "PostgreSQL 설치"
comments: true
categories:
  - DB
tags:
  - PostgreSQL
date: "2020-06-05 10:40"
---

PostgreSQL 설치방법

## PostgreSQL 다운받기

※ PostgreSQL 사이트에서 다운로드 한다.
[https://www.enterprisedb.com/download-postgresql-binaries](https://www.enterprisedb.com/download-postgresql-binaries)


![img](\assets\images\database\postgresql/postgresql_img03.png)

![img](\assets\images\database\postgresql/postgresql_img04.png)
다운이 완료되면 'postgresql.exe' 파일을 실행시킨다.


![img](\assets\images\database\postgresql/postgresql_img05.png)
Next를 누르다보면 설치할 소프트웨어를 선택하는 창이 나온다
'pgAdmin' 은 'postgresql' 서버를 사용할 수 있도록 해주는 GUI 도구이다
이곳에서 함께 깔아도되고 따로따로 깔아도 상관없다


Next를 누르다보면 데이터베이스 관리자 비밀번호를 지정해야 한다
기본값으로 데이터베이스 관리자의 이름은 'postgres' 이다


계속 Next를 누르면 설치가 완료된다


![img](\assets\images\database\postgresql/postgresql_img06.png)
설치가 완료되면 프로세스가 동작한다



## pgAdmin 다운받기


※ pgadmin 사이트에서 다운로드 한다.
[http://pgadmin.org](http://pgadmin.org)


![img](\assets\images\database\postgresql/postgresql_img08.png)
설치파일을 다운받아 실행시킨다


![img](\assets\images\database\postgresql/postgresql_img09.png)
설치가 모두 완료되면 위와 같은 페이지가 열린다


![img](\assets\images\database\postgresql/postgresql_img10.png)
'postgresql' 을 설치할 때 설정한 비밀번호를 입력한다


![img](\assets\images\database\postgresql/postgresql_img11.png)
'pgAdmin4' 의 특징은 자체적으로 웹 서버를 실행하고, 그 웹 서버를 브라우저로 접속해서 데이터베이스를 조작한다
브라우저가 실행되면 'pgAdmin' 웹프로그램을 실행하는 url을 호출하여 마치 홈페이지에 접속하는 것처럼 진행된다
