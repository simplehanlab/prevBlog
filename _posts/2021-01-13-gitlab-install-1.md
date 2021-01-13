---
title: "CentOS 7 Gitlab 설치 및 설정(1)"
comments: true
categories:
  - linux
tags:
  - gitlab
date: "2021-01-13 12:45"
---
### CentOS 7 Gitlab 설치 및 설정(1)

OS : CentOS 7

VERSION : gitlab-ce (community edition - 무료버전)



#### 1. Gitlab 설치시 필요한 패키지 설치

```shell
sudo yum install -y curl policycoreutils-python openssh-server
sudo systemctl enable sshd
sudo systemctl start sshd
sudo firewall-cmd --permanent --add-service=http
sudo firewall-cmd --permanent --add-service=https
sudo systemctl reload firewalld
```

#### 2. 설치 전/후 의존성 패키지 설치여부 확인

**1. policycoreutils-python**

```shell
yum list installed | grep policycoreutils-python
```

![image-20210113144623259](\assets\images\gitlab\1\image-20210113144623259.png)



  **2. openssh-server**

```shell
yum list installed | grep openssh-server
```

![image-20210113144640124](\assets\images\gitlab\1\image-20210113144640124.png)

**3.  openssh-server 구동 여부 확인**

```shell
systemctl status sshd
```

![image-20210113144737316](\assets\images\gitlab\1\image-20210113144737316.png)

**4. 메일링 서비스를 위한 postfix 설치(필요시)**

```shell
sudo yum install postfix
sudo systemctl enable postfix
sudo systemctl start postfix
```



#### 3. Gitlab 설치하기

1. Gitlab-ce Package repository 추가

   ```shell
   curl https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.rpm.sh | sudo bash
   ```

2. Gitlab 을 접속하기 위한 url 설정 후 gitlab-ce 설치

   ```shell
   sudo EXTERNAL_URL="자신의 도메인이나 접속가능한 IP:접속할 Port번호" yum install -y gitlab-ce
   ex) sudo EXTERNAL_URL="https://localhost:8080" yum install -y gitlab-ce
   ```

3. 설치 완료후 EXTERNAL_URL 에 설정된 주소로 접속 후 Gitlab 페이지 확인 

4. 초기 페이지 에서 root 의 패스워드를 새로 지정하기 위한 페이지가 나온다. 