---

title: "워드프레스(wordpress) 설치형 사용법"
comments: true
categories:
  - web
tags:
  - WordPress
date: "2020-07-14 18:45"
---

# 워드프레스 설치형 사용법

## WordPress 설치형 사용법
 
설치형 워드프레스를 쉽게 사용하기위한 플랫폼들이 많은데

• [WAMP](http://www.wampserver.com/en/)

• [MAMP](http://www.mamp.info/en/index.html)

• [LAMP](http://bitnami.org/stack/lampstack)

• [XAMPP](http://www.apachefriends.org/en/xampp.html)

전부 한국어를 지원하지 않으므로, 국내 개발된 autoset을 활용한 워드프레스 사용법입니다.

• [AutoSet](http://autoset.net/xe/download_autoset_10_7_2)

![img](\assets\images\wordpress\b1.png)

 
운영체제 시스템에 따라 알맞게 다운로드 합니다.

다른 아파치서버나 mysql 서버와 충돌이 날 수 있으므로 종료 후 설치를 진행합니다. 

![img](\assets\images\wordpress\b2.png)

![img](\assets\images\wordpress\b3.png)

![img](\assets\images\wordpress\b4.png)

![img](\assets\images\wordpress\b5.png)

설치가 완료 된 후 오토셋을 실행시 다음과 같은 화면이 나옵니다. 

![img](\assets\images\wordpress\b6.png)

기본으로는 웹 서버와 mysql 시작이라고 나올것인데 시작해주시면 됩니다.

그후 제어탭을 눌러 phpMyAdmin에 접속합니다. 

![img](\assets\images\wordpress\b7.png)

초기화면에서 기본값으로 로그인 할 경우

```
//기본값
사용자명 : root
패스워드 : autoset 
```

![img](\assets\images\wordpress\b8.png)

다음과 같은 화면이 나오는데,  

해당화면에서 데이터베이스에 들어가 wordpress에서 사용될 DB를 생성합니다.

![img](\assets\images\wordpress\b9.png)

그리고 워드프레스 최신버전을 설치해줍니다.

[워드프레스 다운로드](https://ko.wordpress.org/download/)

![img](\assets\images\wordpress\b10.png)
 
압축파일을 오토셋의 public_html 폴더에 해제해줍니다.

![img](\assets\images\wordpress\b11.png)

그 후 localhost/wordpress 에 접속하여 워드프레스 환경설정을 해줍니다.

![img](\assets\images\wordpress\b12.png)

![img](\assets\images\wordpress\b13.png)

![img](\assets\images\wordpress\b14.png)
 
또한 설치형은 플러그인을 무료로 사용 가능합니다.

![img](\assets\images\wordpress\b15.png)

![img](\assets\images\wordpress\b16.png)

![img](\assets\images\wordpress\b17.png)

이러한 식으로 플러그인을 찾아 추가해 사용 할 수 있습니다.
