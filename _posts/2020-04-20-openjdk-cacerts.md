---

title: "Open JDK 사용 시 Https 접속 시 에러 해결"
comments: true
categories:
  - java
tags:
  - java
  - jdk
  - linux
  - ubuntu
  - openjdk
date: "2020-04-20 14:00"
---

Oracle에서 JDK 유료화를 진행하려 함에 따라 open jdk 의 활용이 높아지고 있다.

Open JDK 를 통해 Https 통신 요청을 하게 되면  

[javax.net.ssl.SSLException:](javax.net.ssl.sslexception:) [java.lang.RuntimeException:](java.lang.runtimeexception:) Unexpected error: java.security.InvalidAlgorithmParameterException: the trustAnchors parameter must be non-empty

에러가 발생할 때가 있다 .

Truststore라는 인증서들을 보관하는 파일(JRE/lib/security/cacerts)이 있는데, 파일 내부에 해당 https 요청을 할 때 사용할 인증서가 들어있지 않거나 혹은 이 인증서가 유효하지 않아서 나는 에러

이 에러는 특히 OpenJDK를 사용할 때 발생하며 Sun, Oracle, Apple의 JDK를 사용하면 발생하지 않는다.

해결 법은 open jdk의 cacerts 파일을 oracle jdk 의 cacerts 파일로 심볼릭 링크를 걸어 주거나. 파일을 교체해 주면 된다.

이 포스트에서는 파일 교체를 통해 문제를 해결하는 법을 소개한다. 

Openjdk 경로/jre/lib/security 경로의 폴더를 보게 되면 cacerts 라는 파일이 있다. 리눅스 명령어 ls-l 을 통해 내용을 확인해 보면

![](D:\Node.js\workspace\simplehanlab.github.io\assets\images\openjdk-cacerts\cacerts_info.png)



※현재 cacerts 교체를 한 상태로 기존 cacerts 파일은 cacerts_bak 이다.

파일 용량이 32 바이트로 거의 비어 있는 파일이 존재 할 것이다. 이 파일을 기존 Oracle JDK 파일로 교체를 해주어야 한다. 

Oracle JDK 홈 디렉토리/jre/lib/security 폴더 내부에도 같은 파일(cacerts) 가 있을 것이다. 

이 파일을 Open JDK 내부의 cacerts 파일 대신에 사용하면 된다. 

나중을 대비해 Open JDK 의 cacerts 파일을 백업해 두고 작업을 하도록 하자 

※ 리눅스 파일 복사 명령어 : 

- cp ‘복사할 파일경로(상대/절대)’ ‘복사한 파일을 붙여넣을 경로/붙여넣을 때 파일 명(상대/절대)’

```
cp .../jdk180.../jre/lib/security/cacerts .../java-se-8u40-ri/jre/lib/security/cacerts
```

파일 변경 뒤에 톰캣을 재시작 한뒤 테스트해보면 된다.







