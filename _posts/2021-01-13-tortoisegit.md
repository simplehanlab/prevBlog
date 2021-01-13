---

title: "TortoiseGit 설치 및 사용법"
comments: true
categories:
  - web
tags:
  - TortoiseGit
date: "2021-01-13 14:00"
---

# ※	TortoiseGit 설치 및 사용방법

## ●	TortoiseGit 이란

윈도우에서 GUI환경을 이용해서 Git을 제어할 수 있도록 도와주는 버전관리 시스템
때문에 Git은 먼저 설치되어 있어야 한다

 
## ●	Git 설치방법

⊙ Git SCM에 접속하여 설치 파일 다운로드

![img](\assets\images\tortoisegit\tortoise01.png)

주소 : https://git-scm.com/




![img](\assets\images\tortoisegit\tortoise02.png)

![img](\assets\images\tortoisegit\tortoise03.png)





![img](\assets\images\tortoisegit\tortoise04.png)

![img](\assets\images\tortoisegit\tortoise05.png)

Git을 사용할 기본 에디터 선택 (기본설정 Vim)





![img](\assets\images\tortoisegit\tortoise06.png)

![img](\assets\images\tortoisegit\tortoise07.png)

https 전송 인증서 선택 – 기본설정으로 Next




![img](\assets\images\tortoisegit\tortoise08.png)

![img](\assets\images\tortoisegit\tortoise09.png)

OpenSSL 라이브러리 사용 – 기본설정으로 Next




![img](\assets\images\tortoisegit\tortoise10.png)

Git Bash 터미널 에뮬레이터 설정
Use MinTTY(the default terminal of MSYS2) – Git bash 기본 터미널 사용





![img](\assets\images\tortoisegit\tortoise11.png)

![img](\assets\images\tortoisegit\tortoise12.png)

기본설정으로 Next





![img](\assets\images\tortoisegit\tortoise13.png)

![img](\assets\images\tortoisegit\tortoise14.png)

Enable file system caching – 빠른 실행을 위해 데이터를 메모리에 캐시
설정이 완료되면 인스톨






![img](\assets\images\tortoisegit\tortoise15.png)

Launch Git Bash에 체크하고 Finish 하면 Git Bash가 실행된다
특정 폴더에서 우클릭시에도 Git Bash를 실행할 수 있다




## ●	TortoiseGit 설치방법

1.	TortoiseGit 다운로드

Windows 환경에 맞는 설치 파일을 다운로드
다운로드 : https://tortoisegit.org/download/

![img](\assets\images\tortoisegit\tortoise16.png)




2.	설치

![img](\assets\images\tortoisegit\tortoise17.png)

![img](\assets\images\tortoisegit\tortoise18.png)

Tortoise에서 제공하는 기본 PuTTY 기반의 SSH를 선택





![img](\assets\images\tortoisegit\tortoise19.png)

![img](\assets\images\tortoisegit\tortoise20.png)

설치 준비가 완료되면 Install 진행






![img](\assets\images\tortoisegit\tortoise21.png)

첫번째 언어선택 화면에서 사용 언어를 선택
추가할 언어팩은 아래 주소에서 추가할 수 있다
다운로드 : https://tortoisegit.org/download/






![img](\assets\images\tortoisegit\tortoise22.png)

Git의 실행파일 위치를 입력해야 한다. Git을 먼저 설치하였다면 자동으로 등록된다






![img](\assets\images\tortoisegit\tortoise23.png)

유저 정보 입력





![img](\assets\images\tortoisegit\tortoise24.png)

설치 완료





# ●	TortoiseGit 사용방법

1.	데이터 받아오기 (Git 데이터 복제)

![img](\assets\images\tortoisegit\tortoise25.png)

Git 자료를 복제 받기 원하는 곳으로 이동한 후 마우스 우클릭 “Git 복제하기..” 클릭




![img](\assets\images\tortoisegit\tortoise26.png)

새로 생긴 복제창에서 “URL” 부분에 복제할 자료의 Repository 주소를 입력한다





![img](\assets\images\tortoisegit\tortoise27.png)

권한을 가진 Git 사용자 정보를 입력한다






![img](\assets\images\tortoisegit\tortoise28.png)

Git 데이터 복제






![img](\assets\images\tortoisegit\tortoise29.png)

Git 데이터 복제 완료





2.	데이터 업데이트 (Git 데이터 동기화)

![img](\assets\images\tortoisegit\tortoise30.png)

데이터 복제된 폴더에서 우클릭 “Git 동기화..” 클릭







![img](\assets\images\tortoisegit\tortoise31.png)

“가져와 병합하기” 클릭







![img](\assets\images\tortoisegit\tortoise32.png)

업데이트 된 내역이 출력됨. 다시 “가져와 병합하기” 클릭






![img](\assets\images\tortoisegit\tortoise33.png)

데이터가 업데이트 되고 변경된 내역이 출력됨






3.	데이터 저장 (Git 작업 Commit)

![img](\assets\images\tortoisegit\tortoise34.png)

수정한 파일이 위치한 폴더에서 우클릭 “Git 커밋 -> master” 클릭







![img](\assets\images\tortoisegit\tortoise35.png)

작업 후엔 항상 “Commit” 을 해야한다.
메시지 부분에 자신이 작업한 내용을 간략히 기입한다
하단의 파일 리스트에서 자신이 작업한 파일들을 선택하고 커밋을 누른다






![img](\assets\images\tortoisegit\tortoise36.png)


커밋 완료. 위 상태에서는 본인 PC에만 저장되고, 서버에는 올라가지 않은 상태이다






![img](\assets\images\tortoisegit\tortoise37.png)

작업한 폴더에서 우클릭 “TortoiseGit → Git Push” 선택






![img](\assets\images\tortoisegit\tortoise38.png)

로컬, 원격 모두 master 인지 확인 후 확인버튼 선택






![img](\assets\images\tortoisegit\tortoise39.png)

서버에 작업내용 “Push” 완료