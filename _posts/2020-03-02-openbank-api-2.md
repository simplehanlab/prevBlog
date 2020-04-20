---
title: "오픈뱅킹 서비스 API 통신 - 2 (사용자 인증)"
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

#### **사용자 인증**

- 이전 포스트에서 간편 가입 이후 등록한 앱에서 확인한 앱키와 앱 시크릿 키를 통한 사용자 인증을 진행해 보도록 하겠다.

- 사용자에 대한 이용기관의 자체 인증이 없다면 오픈뱅킹에서 제공하는 본인인증 절차를 통해 본인 확인 및 계좌 등록이 가능하다. 

- OAuth2 기반의 인증 절차로 이루어 지며 자체 인증이 없는 경우 3-legged 방식을 통해 오픈뱅킹에서 제공하는 화면을 통해 인증을 진행한다. 

- REST API 를 통해 진행할 수 없으며 웹 또는 앱 어플리케이션에서 인증을 위한 주소를 호출하여 인증화면을 제공 받는다. 

- 이번 테스트에서는 스프링부트 웹 프로젝트를 통해 사용자 인증을 받는 방식을 설명하도록 하겠다. 

- 간단히 테스트를 위한 스프링 부트 웹 프로젝트를 생성한 뒤 JSP 내에서 윈도우 팝업 호출을 통한 인증 페이지를 호출한다. 

**오픈뱅킹에서 제공하는 API 명세서 중 본인인증 요청 API 확인**

![img](\assets\images\openbank\post_2\openbank_2_1.png)

URL : https://openapi.openbanking.or.kr/oauth/2.0/authorize

Method : GET

의 주소를 통한 JSP 내의 form 태그를 이용한 윈도우 팝업 호출 

**form 코드**

```jsp
<form id="authorizeFrm" name="authorizeFrm" method="get" action="https://testapi.openbanking.or.kr/oauth/2.0/authorize" target="_authForm">
    <input type="hidden" name="client_id" value="AyEoyDUkvMZznIdzL1pGdFxTL1rQR2q2OwdJYQMv"/>
    <input type="hidden" name="scope" value="login inquiry transfer"/>
    <input type="hidden" name="redirect_uri" value="http://localhost:8082/bank/callback"/>
    <input type="hidden" name="auth_type" value="0"/>
    <input type="hidden" name="response_type" value="code"/>
    <input type="hidden" name="state" value="12345678901234567890123456789012"/>
</form>
```

**client_id** : 앱 등록 후 발급받은 앱 키 

**scope** : Access Token 권한 범위 (다중 scope 가능)

![img](\assets\images\openbank\post_2\openbank_2_2.png)

**redirect_uri** : 마이페이지 > 내앱관리 > 추가정보 에서 사용자 인증 이후 redirect 될 uri 를 설정 후 해당 주소 사용

![img](\assets\images\openbank\post_2\openbank_2_3.png)

**auth_type** : 

**(0)최초인증** : 처음 오픈뱅킹에 접속하는 사용자의 인증 처리 (본인인증+조회/출금동의) 

~~(1)재인증 : 최초 접속 이후에 추가로 계좌등록 필요시 인증 처리 (본인재인증+ARS동의) 선택항목을 파라미터로 받은 경우 인증화면 입력란에 자동으로 정보 셋팅~~ 

**(2)인증생략** : 유효한 접속토큰을 이용하여 본인인증 절차 생략 (본인인증생략+조회/출금동의) ARS로 조회/출금동의 시 휴대폰번호는 DB에 저장된 값을 활용 (전자금융업자 등 특정 자격요건을 갖춘 사업자에만 선별적으로 허용)

**response_type** : "code" 로 고정

**state** : CSRF 보안위협에 대응하기 위해 이용기관이 셋팅하는 난수값(32Byte 로 고정)

※ 각 파라미터에 대한 자세한 설명은 [오픈뱅킹 서비스 API 목록](\assets\images\openbank\post_2\api_list.pdf) (Ctrl + click 시 새탭으로 열기) 의 사용자 인증 에서 확인

위의 form 코드를 window.open 함수를 이용한 새창에서 호출 스크립트 코드 

**Javascript 코드**

```javascript
var $authorizeFrm = $('#authorizeFrm');
window.open('', '_authForm');
$authorizeFrm.submit();
```

새창에서 해당 정보를 가지고 호출을 하면 다음과 같은 화면을 볼 수 있다. 

![img](\assets\images\openbank\post_2\openbank_2_4.png)

이후 각 인증절차에 따라 인증을 진행하면 파라미터로 설정한 redirect_uri 를 통해 Access Token을 발급 받기 위한 code(authorization_code) 를 리턴 받을 수 있다.

**※응답 형식**

![img](\assets\images\openbank\post_2\openbank_2_5.png)

redirect_uri 로 설정된 경로에서 code 값을 추출 

![img](\assets\images\openbank\post_2\openbank_2_6.png)

#### **사용자 토큰 발급 ** (3-legged)

**<사용자 토큰 발급 API 설명서>**

![img](\assets\images\openbank\post_2\openbank_2_7.png)

추출한 code 값을 이용하여 토큰 발급 API 요청을 진행한다. 

**<서버 소스 코드>**

```java
HashMap<String, String> authHeaders = new HashMap<>();
    authHeaders.put("content-type", "application/x-www-form-urlencoded; charset=UTF-8");

    HashMap<String, Object> authParams = new HashMap<>();
    authParams.put("code", reqParams.get("code"));
    authParams.put("client_id", clientId);
    authParams.put("client_secret", clientSecret);
    authParams.put("grant_type", "authorization_code");
    authParams.put("redirect_uri", redirectUri);


    JSONObject authRes = HttpUtils.httpPost("https://testapi.openbanking.or.kr/oauth/2.0/token", authHeaders, authParams);
    String accessToken = (String) authRes.get("access_token");
    String refreshToken = (String) authRes.get("refresh_token");
    String userSeqNo = (String) authRes.get("user_seq_no");
    String scope = (String) authRes.get("scope");
    String tokenType = (String) authRes.get("token_type");
```
