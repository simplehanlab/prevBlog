---

title: "MySQL 설치"
comments: true
categories:
  - DB
tags:
  - MySQL
date: "2020-06-04 16:37"
---

MySQL 설치방법


## MySQL 다운받기

※ MySQL 사이트에서 다운로드 한다.

[http://www.mysql.com/](http://www.mysql.com/)

![img](\assets\images\database\download01.png)
DOWNLOADS 탭 하단부 MySQL Community (GPL) Downloads 클릭

![img](\assets\images\database\download02.png)
MySQL Installer for Windows 클릭

![img](\assets\images\database\download03.png)
아래쪽 Download 클릭

![img](\assets\images\database\download04.png)
바로 설치를 진행한다


## MySQL 설치하기

※ 설치 전 필요에 맞게 설정 후 설치를 진행한다.

![img](\assets\images\database\download05.png)
먼저 설치타입을 설정한다

![img](\assets\images\database\download06.png)
두번째 'Check Requirements' 에서 MySQL 을 설치할 때 추가로 설치할 프로그램을 선택한다
'Execute' 를 누르면 자동으로 설치한다

![img](\assets\images\database\download07.png)
'Execute' 을 누르면 위에서 설치한 프로그램들을 인스톨 한다

![img](\assets\images\database\download08.png)
'Accounts and Roles' 에서 'Root' 계정의 비밀번호를 설정한다

![img](\assets\images\database\download09.png)
서비스 항목에 저장될 이름을 지정한다

![img](\assets\images\database\download10.png)
'Execute' 을 눌르면 서비스 등록 및 MySQL서버 시작 등 항목을 차례대로 진행한다

![img](\assets\images\database\download11.png)
'Check' 버튼을 눌러서 설치한 MySQL서버가 제대로 동작하는지 테스트 할 수 있다

![img](\assets\images\database\download12.png)
설치를 완료한다

![img](\assets\images\database\download13.png)
위 과정이 이상없이 성공적으로 진행 되었다면 자동으로 'Shell' 과 'MySQL Workbench' 가 실행된다


## MySQL 한글(UTF8) 설정하기

※ 설치 전 필요에 맞게 설정 후 설치를 진행한다.

## 데이터셋 설정 확인

#### [MySQL Workbench 확인]

1. 'Navigator' 목록에서 'Status and System Variables' 를 선택한다
2. 오른쪽탭(System Variables)을 선택하고 'character' 를 검색한다

![img](\assets\images\database\download14.png)
위와 같이 실행하면 현재 설정값을 알 수 있다.

만약 설정값이 utf8이 아니라면 'my.ini' 파일을 수정해야 한다
('my.ini' 파일 위치는 위의 결과값 맨 마지막 행의 dir 위치를 참고)

![img](\assets\images\database\download15.png)
'my.ini' 파일 안에 위 내용을 넣어주고 'MySQL' 을 재시작한다

![img](\assets\images\database\download16.png)
다시 workbench로 확인하면 utf8로 설정된 것을 볼 수 있다


#### [MySQL 콘솔 확인]


![img](\assets\images\database\download17.png)
MySQL 콘솔창에 'status'명령어를 입력하여 'characterset' 을 확인

![img](\assets\images\database\download18.png)
'show variables like 'char%';' 명령어를 실행시켜 'characterset' 관련된 변수를 출력하여 확인


이 경우에도 위와 같은 방법으로 my.ini 파일을 추가해주면 해결된다