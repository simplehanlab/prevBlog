---

title: "TortoiseGit 사용방법"
comments: true
categories:
  - web
tags:
  - TortoiseGit
date: "2021-01-13 15:30"
---

# ※	TortoiseGit 사용방법


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