---
title: "CentOS 7 Gitlab 설치 및 설정(2) - Gitlab 설정"
comments: true
categories:
  - linux
tags:
  - gitlab
date: "2021-01-13 14:45"
---
### CentOS 7 Gitlab 설치 및 설정(2) - Gitlab 설정

###### OS : CentOS 7
###### VERSION : gitlab-ce (community edition - 무료버전)

###### [CentOS 7 Gitlab 설치 및 설정(1) - Gitlab 설치하기](https://simplehanlab.github.io/linux/gitlab-install-1)

#### 1. External url 설정 변경

###### 설정 파일 위치 : /etc/gitlab/gitlab.rb

```shell
sudo vim /etc/gitlab/gitlab.rb
```

###### : 설정 파일 내에서 external_url 부분을 찾아서 원하는 url 로 변경

![image-20210113154112679](\assets\images\gitlab\2\image-20210113154112679.png)

#### 2. 저장소 데이터 저장 위치 변경 

###### 설정 파일 위치 : /etc/gitlab/gitlab.rb

```shell
sudo vim /etc/gitlab/gitlab.rb
```

###### : 설정 파일 내에서 git_data_dirs 경로 변경

![image-20210113154201441](\assets\images\gitlab\2\image-20210113154201441.png)

#### 3. SMTP 설정 

###### 설정 파일 위치 : /etc/gitlab/gitlab.rb

```shell
sudo vim /etc/gitlab/gitlab.rb
```

###### : SMTP 를 이용한 메일링 서비스 이용을 할 경우 파일 내의 SMTP 설정 부분 변경

![image-20210113154428321](\assets\images\gitlab\2\image-20210113154428321.png)

※Gitlab SMTP 설정 관련 공식 문서 : [Gitlab SMTP 설정 관련 Docs](https://docs.gitlab.com/omnibus/settings/smtp.html)

#### 4. Gitlab 변경된 설정 적용하기

```shell
sudo gitlab-ctl stop				// gitlab 중지
sudo gitlab-ctl reconfigure			// 설정 적용
sudo gitlab-ctl restart				// gitlab 재시작
```