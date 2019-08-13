---
title: "minimal-mistakes에 Google Analytics 연동하기"
comments: true
categories:
  - jekyll
  - minimal-mistakes
tags:
  - github pages
  - jekyll
  - minimal-mistakes
  - Google Analytics
date: "2019-08-09 15:00"
---
블로그를 운영하는데 통계가 필요하다. 다행히 minimal-mistake 테마에서 Google Analytics 연동을 지원한다. 그래서 Google Analytics와 블로그를 연동해보기로 했다. (Google Analytics 외에 다른 통계 소스 연동도 지원하고 커스텀을 하는 방법도 지원한다. 커스텀을 이용하는 방법은 아직 해본 적이 없다.)

일단 [Google Analytics 사이트]로 이동하자.

[Google Analytics 사이트]: https://analytics.google.com/analytics/web/

처음 접속을 하면 "무료로 이용하기" 와 같은 글이 떠있는 것을 보니 무료인 것 같다. *(해당 첫 접속 페이지 이미지를 깜빡하고 캡처하지 못해서 남기질 못했다. 가입 후에는 나오질 않아서 추후 이미지를 얻게 되면 업로드 예정.)*

![](\assets\images\google-analytics\create-account.jpg)

계정 가입 시 첫번째로 위와 같이 계정 이름을 입력해줘야 한다. 이 계정 이름은 자신이 원하는 대로 지정해주자. 그 후 자신에게 필요한 옵션을 아래 체크박스에서 체크한 뒤 **다음**을 눌러주자.

![](\assets\images\google-analytics\create-account-2.jpg)

다음으로 자신의 측정 대상을 선택해주자. 나는 웹 기반의 이 블로그를 측정할 것이기 때문에 **웹**을 선택했다.

![](\assets\images\google-analytics\create-account-3.jpg)

마지막으로 자신의 측정 대상의 정보를 입력해주자. 나는 웹을 선택했기 때문에 위와 같이 나오는데 다른 대상을 선택했으면 다른 세부정보 입력 화면이 나오니 잘 읽고 입력해주자.

![](\assets\images\google-analytics\create-account-4.jpg)

가입이 완료되면 위와 같이 ***추적 id (tracking id)***를 확인할 수가 있는데 이를 복사해두자.

그 후 minimal-mistakes 기반 블로그의 _config.yml 파일을 열면 analytics 항목이 있는데 아래와 같이 작성해주자.

```yml
analytics:
  provider: "google"
  google:
    tracking_id: "위에서 복사한 추적 id"
    anonymize_ip: false # true, false (default)
```

그 후 블로그에 설정을 반영한 뒤 analytics 사이트에서 통계가 제대로 집계되고 있는 지 확인해보자.

![](\assets\images\google-analytics\analytics-home.jpg)

제대로 집계가 된다면 블로그 접속 시 analytics 홈에 위와 같이 현재 활성 사용자가 카운트된다. 집계가 되는 것이 확인되면 좌측 메뉴에서 여러가지 통계를 볼 수 있으니 잘 활용해보자.

## FAQ

### Q1 : 추적 ID를 어디서 확인하는 지 모르겠어요.

추적 ID는 기본적으로 최초에 계정을 생성하면 생성 완료 후 보여주게 되는데, 이를 확인하지 못했거나 웹사이트 에러, 브라우자 에러 등등 모종의 이유로 확인을 하지 못했을 시를 대비해서 추후에 확인할 수 있는 방법이 있다. 아래 이미지를 보고 추적 ID를 확인하자.

![](\assets\images\google-analytics\tracking-id.jpg)

1. 좌측 하단의 **관리** 클릭.
2. **속성** 탭 확인
3. **추적 정보** 탭의 **추적 코드** 클릭.
4. 상단의 **추적 ID** 확인.