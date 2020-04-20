---
title: "오픈뱅킹 서비스 API 통신 - 1"
comments: true
categories:
  - etc
  - openbanking
tags:
  - RestAPI
  - openbanking
  - 오픈뱅킹
date: "2020-02-28 18:00"
---

#### **계정 생성(테스트를 위한 간편가입)**

오픈 뱅킹에서는 테스트 이용자를 위한 테스트베드 서비스를 제공한다. 

 [테스트 베드 웹 사이트](https://developers.openbanking.or.kr/ ) https://developers.openbanking.or.kr/  접속 

상단 메뉴 우측 간편가입을 통해 간편가입 진행

![img](\assets\images\openbank\post_1\openbank_1_1.png)



#### **앱 등록 및 앱 키 발급** 

로그인 후 우측 마이페이지 > 내앱 선택 > 내앱 관리에서 앱 등록 후 앱키와 앱 시크릿 키를 발급 받을 수 있다.

![img](\assets\images\openbank\post_1\openbank_1_2.png)

![img](\assets\images\openbank\post_1\openbank_1_3.png)

여기서 발급 받은 앱키와 앱 시크릿 키를 이용하여 사용자 인증시에 요청 파라미터에 사용

#### **테스트 베드 API  콘솔**

테스트 베드에서 제공하는 API 콘솔을 이용하여 각 API 별로 요청 파라미터와 응답 데이터에 대한 간단한 API 테스트가 가능하다. 

![img](\assets\images\openbank\post_1\openbank_1_4.png)

API 의 구조 파악과 통신에 대한 간단한 테스트만 가능 할 뿐 테스트 베드에서 제공하는 테스트 데이터와는 무관한 데이터가 나온다. 

다음 포스트에서 테스트 베드를 통해 제공받은 앱키와 앱 시크릿 키를 이용해 사용자 인증을 진행해보도록 하겠다.







