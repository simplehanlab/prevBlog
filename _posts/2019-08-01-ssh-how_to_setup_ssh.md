---
title: "[Ubuntu 18.04] SSH 접속 설정 하기"
comments: true
categories:
  - SSH
tags:
  - ssh
  - ubuntu
  - linux
date: "2019-08-07 15:00"
---

## SSH 란?

SSH (Secure Shell Protocol) 이란 네트워크 프로토콜중 하나로, 컴퓨터와 컴퓨터가 Public Network를 통해 서로 통신(연결)을 할때 보안적으로 안전하게 사용하기 위한 프로토콜 입니다.

## Ubuntu 18.04 에 SSH 적용하기

#### 1. openssh-server 설치 유무 확인

```bash
$ dpkg -l | grep openssh
```

![](\assets\images\ssh-how_to_setup_ssh\dpkg-openssh.jpg)

※ 우분투를 설치하시게 되면 기본적으로 openssh-client 는 설치가 되어있으실 겁니다. 

#### 2.openssh-server 설치

```bash
$ apt-get install openssh-server
```

![](\assets\images\ssh-how_to_setup_ssh\install-openssh-server.jpg)

#### 3. openssh-server 설치 확인

```bash
$ dpkg -l | grep openssh
```

![](\assets\images\ssh-how_to_setup_ssh\check-install-openssh.jpg)

#### 3.Putty 로 ssh 접속 확인하기

##### - server ip address 확인

```bash
$ ifconfig
```

![](\assets\images\ssh-how_to_setup_ssh\check-ipadress.jpg)

#####  - Putty 접속 확인 

![](\assets\images\ssh-how_to_setup_ssh\putty.jpg)

우분투 설치시 설정한 계정과 비밀번호를 입력 하면 로그인이 되는 것을 확인할 수 있다.

![](\assets\images\ssh-how_to_setup_ssh\connect.jpg)



