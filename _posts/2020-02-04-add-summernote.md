---
title: "Spring 프로젝트에 Summernote 적용하기"
comments: true
categories:
  - Spring
tags:
  - Spring
  - STS
  - JSP
  - Summernote
date: "2020-02-04 18:00"
---

이전에 작성한 Naver SmartEditor 포스트에 이어서 이번엔 다른 위지윅 에디터를 적용해보기로 했다. 이번에 적용해 볼 에디터는 **Summernote**라는 에디터이다. bootstrap, jQuery 기반의 에디터여서 필자 기준으로는 SmartEditor보다 더 쉽게 사용을 할 수 있었다. SmartEditor에선 몇번 헤딩을 한 이미지 업로드도 Summernote에선 너무나도 쉽게 구현을 하였다.



이전에 진행한 Bootstrap을 적용한 Spring 프로젝트를 이용해서 진행한다.

[Spring 프로젝트에 Bootstrap 적용하기](/spring/add-bootstrap)



## Summernote 리소스 받기

일단 처음으로 [Summernote 다운로드 페이지](https://github.com/summernote/summernote/releases)에서 리소스를 다운받자. 필자는 zip 파일을 다운받았다.

![add-summernote-2](\assets\images\add-summernote\add-summernote-2.JPG)



다운받은 zip 파일의 압축을 푼 뒤, dist폴더 내부의 파일들을 src/resources/libs/summernote에 넣어주자.

![add-summernote-3](\assets\images\add-summernote\add-summernote-3.JPG)



## 코드 생성

다음으로 summernote용 controller와 view를 작성해보자. 먼저 *com.example.demo.summernote.controller* 패키지를 만들고 *SummernoteController.java* 파일을 만들자.

![add-summernote-4](\assets\images\add-summernote\add-summernote-4.JPG)

```java
package com.example.demo.summernote.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;

@Controller
public class SummernoteController {
	@RequestMapping("/summernote")
	public String summernote() {
		return "summernote";
	}
}
```



그 다음 JSP 파일을 만들자. 이름은 controller에서 지정해준 것 처럼 summernote.jsp로 만들었다.

![add-summernote-5](\assets\images\add-summernote\add-summernote-5.JPG)

```jsp
<%@ page language="java" contentType="text/html; charset=EUC-KR" pageEncoding="EUC-KR"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>

<!DOCTYPE html>
<html>
	<head>
		<meta charset="EUC-KR">
		<link id="contextPath" data-path="<c:url value='/'/>" />
		<link href="https://stackpath.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css" rel="stylesheet">
		<link  rel="stylesheet" href="<c:url value='/libs/summernote/summernote.min.css'/>"/>
		
		<script src="<c:url value='/libs/jquery-3.4.1.min.js'/>"></script>
		<script src="https://stackpath.bootstrapcdn.com/bootstrap/3.3.7/js/bootstrap.min.js"></script>
		<script src="<c:url value='/libs/summernote/summernote.min.js'/>"></script>
		<script src="<c:url value='/js/summernote/summernote.js'/>"></script>
		
		<title>Insert title here</title>
	</head>
	<body>
		<div id="summernote">Hello Summernote</div>
	</body>
</html>
```



**유심히 봐야할 부분이 있다. 그간 포스트 작성하며 사용해 온 bootstrap은 4버전인데, summernote는 bootstrap 3버전에서 운용해줘야 깔끔하게 돌아간다. 이에 bootstrap css와 js 부분을 CDN을 통해서 3.3.7 버전을 내려받아서 사용하였다. 만약 3.3.7을 따로 가지고 있는 사람들은 해당 리소스를 사용하면 될 것이다.**

그리고 추가로 *src/resources/static/js/summernote/summernote.js* 파일을 추가한 뒤 아래 코드를 작성하자.

```javascript
$(document).ready(function() {
	$('#summernote').summernote();
});
```



이제 브라우저에서 http://localhost:8080/summernote로 접속하여 summernote 에디터가 잘 뜨는지 확인해보자.

![add-summernote-6](\assets\images\add-summernote\add-summernote-6.JPG)



summernote는 이미지 업로드 시 해당 이미지를 base64로 변환한 뒤 바로 코드로 적용하도록 동작한다. 완전히 클라이언트 단에서 모든 작업이 이루어지게 되어 있는데, 이를 이대로 사용할 수도 있지만 서버에 작성한 HTML 코드를 전송했을 때 base64의 어마어마한 텍스트가 같이 전달된다. 이미지를 10개, 20개 추가한 뒤 서버로 보내게 되면 텍스트의 양은 어마어마해져서 이를 조금 수정하려고 한다.

작동 방식을 다음과 같이 바꿔봤다.

1. 에디터에서 이미지 선택
2. 서버에 이미지를 전송하여 서버에 이미지 저장
3. 에디터 코드에서 img 태그를 사용하여 업로드 한 서버 이미지 호출

문제는 이렇게 코드를 적용시키면 추후에 이미지를 추가했다가 지워버리면 해당 이미지가 temp로 남아버리기 때문에 temp 파일 관리가 추가로 들어가줘야 한다.

일단은 위 로직부터 구현을 해보자.



## 파일 업로드 & 조회 로직 구현

위 로직을 구현하려면 파일 업로드와 파일 조회 로직을 구현해야 한다. 이건 추후 포스팅으로?