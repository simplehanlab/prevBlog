---
title: "인스타그램 최근 이미지 피드하기"
comments: true
categories:
  - react
tags:
  - javascript
  - react
  - node
  - instagram
  - feed
date: "2019-11-28 18:39"
---

### 1. 인스타그램 피드하기

  시작하기에 앞서, 인스타그램의 Basic Display API 지원 정책 변경으로 
  
  10월 15일부터 신규 Client 등록 및 허가가 중단되어

  다른 방법을 알아보다가 알게된 방법입니다.

  사용법은 매우 간단합니다.

  먼저 [Lightwidget](https://lightwidget.com/){:target="_blank"} (https://lightwidget.com/) 사이트에 접속하여

  인스타그램 계정으로 로그인한 후 액세스 권한을 받아옵니다.

  ![img](\assets\images\insta\instagram-token.png)

  그 후 아래와 같이 보이는 페이지에서 원하는 값을 설정 합니다.

  ![img](\assets\images\insta\2019-11-28 185648.png)

  아래로 내려 GetCode!를 누르게 되면,

  사용할수있는 HTML iframe 태그를 주게 됩니다.

  ![img](\assets\images\insta\last.png)

  ![img](\assets\images\insta\code.png)

  ![img](\assets\images\insta\2019-11-29 110155.png)

