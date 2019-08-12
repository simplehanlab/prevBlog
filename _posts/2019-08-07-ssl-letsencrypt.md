---
title: "Letsencrypt + Nginx SSL인증서(HTTPS) 적용"
comments: true
categories:
  - SSL
tags:
  - https
  - SSL
  - letsencrypt
date: "2019-07-30 11:00"
typora-root-url: ..\assets\images\letsencrypt
---
### 환경

- Ubuntu 18.04
- apt-get 을 통해 설치한 Nginx
- 도메인 ( 이번 과정에서는 도메인이 없는 경우를 대비 ngrok 을 이용한 임시 도메인 사용 )

## Let's Encrypt

- 웹 사이트에 HTTPS (SSL / TLS)를 사용하기 위해 필요한 디지털 인증서를 무료로 제공
- 만료 기간이 90일, 만료 기간전에 수동으로 인증서를 갱신 또는 crontab, certbot auto 를 활용한 자동 갱신
- 자세한 내용은  [ Let's Encrypt 공식 사이트](https://letsencrypt.org/about/) 에서 확인 하시기 바랍니다.

## Nginx 설치

```bash
$ sudo apt-get install nginx
```

80 포트를 사용하는 또다른 서비스가 존재한다면 서비스를 중지하거나 삭제 한 후 nginx 설치

설치완료 후 서버  포트 번호로 웹 브라우저에서 접속 시 nginx 화면이 나오는지 확인한다. 

![img](\assets\images\letsencrypt\start-nginx.jpg)

Nginx 화면이 나오지 않을 시에는

```bash
$ sudo service nginx restart
```

명령어로 nginx 를 재시작 해본 뒤 다시 웹 브라우저에서 접속시도 nginx index 화면이 제대로 나온다면 이제 Let's Encrypt 를 이용하여 SSL 인증서를 받아보자!!

## certbot  설치

apt-get 에 certbot 저장소 추가

```bash
$ sudo add-apt-repository ppa:certbot/certbot
```

패키지 목록을 업데이트 한 후에 certbot 을 설치

```bash
$ sudo apt-get update
$ sudo apt-get install python-certbot-nginx
```



SSL 인증을 하기에 앞서 구매한 도메인이 없다면, certbot 에서 해당 서버에 접근을 할 수가 없어 인증이 되지 않는다. 잠시 테스트용으로 ngrok 을 사용하여 임시 도메인을 사용해 테스트를 해보자!

## Ngrok 설치 및 실행 

 [ngrok 홈페이지](https://dashboard.ngrok.com/get-started)에 들어가 간단한 회원가입 절차를 진행한 후에 운영체제에 맞는 ngrok 을 다운 받은 뒤 매뉴얼에 따라 ngrok을 실행해 보자 (저는 ubuntu 64bit 여서 linux 를 받았습니다.) 

![img](\assets\images\letsencrypt\download-ngrok.jpg)

③ 번 까지 진행을 했다면 ngrok을 압춘을 푼 폴더에서 

```bash
$ sudo ./ngrok http http://localhost
```

를 실행 해보자 그러면 이러한 화면이 나올 것이다. 

![img](\assets\images\letsencrypt\exec-ngrok.jpg)

Fowarding 부분에 있는 http 주소를 복사( CTRL + C 는 ngrok을 중지시키는 단축키이므로 GUI 가 있는 운영체제라면 마우스 오른쪽 클릭을 활용하거나 직접 주소를 타이핑 하자!!!) 하여 웹 브라우저에서 들어가보면 nginx index 창이 뜨는걸 확인 할수 있다. 

이제 ngrok을 실행 시켜 놓은 상태에서 새로운 터미널 창을 연뒤 certbot 을 이용해 인증서를 발급해 보도록 하자

## Let's Encrypt 인증서 발급

설치한 certbot 을 이용하여 인증서를 발급 받을 것이다. 여기에선 인증서만 필요하므로 certonly 형식으로 인증서만 서버에 받아 오도록 진행 해보자

```bash
$ sudo certbot certonly --nginx -d [도메인 주소]
```

저 도메인 주소에 ngrok 으로 생성한 도메인을 넣어 주도록 하자 

```bash
$ sudo certbot certonly --nginx -d 8e7dcf95.ngrok.io
```

도메인 주소 부분에 http 또는 ' / ' 가 포함되지 않도록 주의 하자

명령어를 실행하면 서버 인증이 실행 되고 

![img](\assets\images\letsencrypt\ssl-certbot.jpg)

Congratulation! 이라는 단어와 함께 인증이 완료되었다고 나올 것이다. 

인증 완료 문구에서 보면 

```shell
/etc/letsencrypt/live/8e7dcf95.ngrok.io/fullchain.pem
/etc/letsencrypt/live/8e7dcf95.ngrok.io/privkey.pem
```

같은 경로가 보일 텐데 이 경로가 인증서가 저장되어진 경로이다. 해당 경로로 들어가 인증 된 파일을 확인해 보자

![img](\assets\images\letsencrypt\certfile.jpg)

총 4가지의 파일이 존재 할 것이다. 이중 fullchain.pem 과 privkey.pem 두가지만 nginx SSL 설정에 사용할 것이다. 

## Nginx SSL 설정

Nginx 의 설정 파일을 수정 ( 여기서는 /etc/nginx/site-available/default 파일을 수정)

```bash
$ sudo vim /etc/nginx/site-available/default
```

파일 내부를 확인해 보면 80 port 설정에 대한 server 블록이 존재할 것이다. 그 server 블록 아래에 443(SSL) port 에 대한 server 블록을 추가해 주자 

```shell
server {
	    ...
        listen 80;
        listen [::]:80;
        root /var/www/html;
        index index.html index.htm index.nginx-debian.html;

        server_name _;
        charset utf-8;

        location / {
                # First attempt to serve request as file, then
                # as directory, then fall back to displaying a 404.
                try_files $uri $uri/ =404;
        }
        ...
}
server {
        listen 443;
        listen [::]:443;
        ssl on;
        server_name _;

        ssl_certificate /etc/letsencrypt/live/8e7dcf95.ngrok.io/fullchain.pem;
        ssl_certificate_key /etc/letsencrypt/live/8e7dcf95.ngrok.io/privkey.pem;

        location / {
                # First attempt to serve request as file, then
                # as directory, then fall back to displaying a 404.
                 try_files $uri $uri/ =404;             
        }
}
```

ssl on 을 통해 SSL 을 활성화 시키고 

ssl_certificate 에 발급받은 인증서 중 fullchain.pem 이 위치한 경로를 지정해 주고 

ssl_certificate_key 에 발급받은 인증서 중 privkey.pem 이 위치한 경로를 지정해 준다.

파일을 저장 하고 나온 뒤 nginx 서버를 재시작 하자!

```bash
$ sudo service nginx restart
```

서버 재시작이 정상적으로 완료 되면 ngrok 에서 발급 받은 주소가 아닌 서버 포트 번호를 통해 HTTPS 보안 접속을 시도해 보자 

![img](\assets\images\letsencrypt\connect-https-port.jpg)

연결이 비공개로 설정 되어 있다는 화면이 나올 것이다. 당황하지 말고 하단 고급탭을 선택 후 "192.168.107.128(안전하지 않음)(으)로 이동" 을 클릭해 보자 

![img](\assets\images\letsencrypt\connect-https-port-result.jpg)

nginx index 화면이 정상적으로 나올 것이다. 

여기서 인증서가 적용이 잘 되었는지 확인을 하기 위해 웹 브라우저 주소검색줄 부분 왼쪽을 보면 주의요함 이라는 글씨가 보일 것이다. 이부분을 클릭한뒤 나오는 메뉴 중에서 인증서 탭을 선택해 보자 

![img](\assets\images\letsencrypt\check-cert.jpg)

인증서 정보가 나오게 될 텐데 여기서 발급 대상은 ngrok을 통해 생성한 도메인 주소가, 발급자는 Let's Encrypt 가 유효기간은 90일이 설정되어 보일 것이다. 

 ![img](\assets\images\letsencrypt\check-cert2.jpg)



다음 게시글에서는 SSL 설정이 완료된 Nginx 에 Tomcat 을 연동하는 방법을 설명해 보겠다.

바로가기: [Nginx + Tomcat8 연동](https://simplehanlab.github.io/ssl/ssl-nginx-tomcat/)