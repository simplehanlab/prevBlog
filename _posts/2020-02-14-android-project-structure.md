---
title: "Android 프로젝트 구조"
comments: true
categories:
  - react
tags:
  - javascript
  - react
  - node
  - css
  - redux
date: "2020-02-14 17:35"
---

<!-- ### 1. Android Native App 이란? -->

  <!-- 우리가 흔히 사용하는 모바일 어플리케이션을 Native App 이라고 한다.
  네이티브 앱은 모바일 기기에 최적화된 언어로 개발된 앱으로 기기에서 지원하는 모든 기능을 온전히 사용할 수 있다(카메라, gps 등) -->


### 1. Android 프로젝트 구조

  안트로이드 어플리케이션 프로젝트의 구조는 크게 4가지로 나눌 수 있다.
  <!-- 안드로이드스튜디오 프로젝트 좌측 부분 스크린샷 넣을 것. -->
  ![img](\assets\images\android\basic_structure.png)

    1. manifests - 'AndroidManifest.xml' 안드로이드의 컨트롤 타워라고 할 수 있다. 어플리케이션을 구동하는데 필요한 설정값을 관리해주는 곳이다.
    2. java - 클래스를 관리하는 폴더이다.
    3. res - Resource 폴더로 UI와 관련된 파일과 디자인 리소스, 문자열 리소스를 담고 있는 폴더이다.
    4. bulid.gradle - 어플리케이션을 빌드하기위해 필요한 설정 옵션, 라이브러리 정보들이 들어있다.


  a. AndroidManifest.xml
  <!-- 매니페스트 스크린샷. -->
  ![img](\assets\images\android\androidmanifest.png)

  어플리케이션이 구동하는데 필요한 요소를 'AndroidManifest'에서 추가, 설정하고
  어플리케이션과 장치간에 상호작용하기 위해 필요한 권한 등을 설정한다.

  이곳에서 패키지의 이름을 정의하거나, 어플리케이션의 이름, 화면에 표시될 아이콘 등을 설정할 수 있다.
  또한 어플리케이션에서 사용할 구성요소들의 클래스명을 지정하고
  각 클래스들이 실행될 수 있는 조건을 기술한다.


  b. java 패키지
  <!-- activity 스크린샷. -->
  ![img](\assets\images\android\MainActivity.png)

  '안드로이드를 프로그래밍한다.' 라고 말하는 부분은 이곳을 수정한다는 얘기와 다르지 않다.
  어플리케이션이 작동하는 방식에 대해 정의하는 java 파일은 모두 이곳에 작성한다.

  setContentView(R.layout.activity_main); 의 코드가 해당 java 파일에서 작성한 코드들를
  실행할 화면을 지정하는 것인데, 때문에 기본적으로 'Activity' 파일 하나에 뒤에 설명 할 xml파일이 한쌍이 된다.


  c. activity_main.xml
  <!-- activity_main.xml 스크린샷 -->
  ![img](\assets\images\android\activity_main.png)

  연결된 Activity 에서 작성한 코드가 어떻게 보여지는지 이곳에서 수정한다.
  이곳이 사용자가 보게 될 UI 화면이다.


  프로젝트 구조로 볼 때 기억해야 할 것은 '<Application>' 위에 '<Activity>' 가 실행된다는 것과
  기능은 'Activity' 에서 UI는 'xml' 에서 작성한다는 것, 그리고 '<Activity>' 마다 'xml' 파일이 연결되어 있다는 것이다.
  따라서 새로운 화면을 만들때 'xxx_Activity.java' 파일을 만들고 그 파일에 activity_xxx.xml 파일을 연결해 주면 되는 것이다.