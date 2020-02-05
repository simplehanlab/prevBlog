---
title: "Spring 프로젝트에 Naver SmartEditor 적용하기"
comments: true
categories:
  - Spring
tags:
  - Spring
  - STS
  - JSP
  - SmartEditor
date: "2020-02-04 17:00"
---

네이버 블로그, 카페 등을 이용할 때, 웹상에서 글쓰기를 진행하다보면 폰트설정, 링크넣기, 이미지넣기 등등 부가적인 기능이 다양하게 들어간 에디터를 번번히 사용하게 된다. 이러한 에디터들을 **위지윅 에디터(WYSIWYG Editor)**라고 하며 Naver에선 Smart Editor 2 버전을 무료로 공개중이다. 이번 포스트에서는 해당 Smart Editor를 적용해볼까 한다.



이전에 진행한 Bootstrap을 적용한 Spring 프로젝트를 이용해서 진행한다.

[Spring 프로젝트에 Bootstrap 적용하기](/spring/add-bootstrap)





## SmartEditor 리소스 받기

[SmartEditor github 다운로드 페이지](https://github.com/naver/smarteditor2/releases)에서 배포판을 다운받을 수 있다. **처음엔 zip 파일을 다운받아서 진행했는데 뭔가 파일이 빠져있는 것 같았다. 안에 skin.html에서 경로에 없는 파일을 참조하고 있길래 zip 파일 리소스는 다 지우고 tgz 파일을 다시 받아서 진행했다.**

![add-smarteditor-1](\assets\images\add-smarteditor\add-smarteditor-1.JPG)





tgz 파일의 압축을 풀어보면 package 폴더가 나오고 안에 들어가면 뭔가 파일이 몇개 있는데, 다 무시하고 dist 폴더 안의 내용만 있으면 된다. *(smarteditor2-2.10.0/package/dist)*

![add-smarteditor-2](\assets\images\add-smarteditor\add-smarteditor-2.JPG)





해당 폴더 안의 파일들을 전부 복사해서 src/resources/static/libs/smarteditor 폴더에 넣어주자.

![add-smarteditor-3](\assets\images\add-smarteditor\add-smarteditor-3.JPG)





## 코드 생성

다음으로 smarteditor 용 controller와 view를 작성해보자. 먼저 *com.example.demo.smarteditor.controller* 패키지를 만들고 *SmartEditorController.java* 파일을 만들자.

![add-smarteditor-4](\assets\images\add-smarteditor\add-smarteditor-4.JPG)

```java
package com.example.demo.smarteditor.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;

@Controller
public class SmartEditorController {
	@RequestMapping("/smarteditor")
	public String smartEditor() {
		return "smartEditor";
	}
}
```





그 다음 JSP 파일을 만들자. 이름은 controller에서 지정해준 것 처럼 smartEditor.jsp로 만들었다. js 코드는 간단하기 때문에 jsp 내부 하단에 바로 작성하였다.

![add-smarteditor-5](\assets\images\add-smarteditor\add-smarteditor-5.JPG)

```jsp
<%@ page language="java" contentType="text/html; charset=EUC-KR" pageEncoding="EUC-KR"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>

<!DOCTYPE html>
<html>
	<head>
		<meta charset="EUC-KR">
		<link id="contextPath" data-path="<c:url value='/'/>" />
		<link rel="stylesheet" href="<c:url value='/libs/bootstrap-4.4.1-dist/css/bootstrap.min.css'/>">
		
		<script src="<c:url value='/libs/jquery-3.4.1.min.js'/>"></script>
		<script src="<c:url value='/libs/bootstrap-4.4.1-dist/js/bootstrap.min.js'/>"></script>
		<script type="text/javascript" src="<c:url value='/libs/smarteditor/js/service/HuskyEZCreator.js'/>" charset="utf-8"></script>
		
		<title>Insert title here</title>
	</head>
	<body>
		<textarea name="ir1" id="ir1" rows="10" cols="100">에디터에 기본으로 삽입할 글(수정 모드)이 없다면 이 value 값을 지정하지 않으시면 됩니다.</textarea>
	</body>
	
	<script>
		var oEditors = [];
		nhn.husky.EZCreator.createInIFrame({
			oAppRef: oEditors,
			elPlaceHolder: "ir1",
			sSkinURI: $('#contextPath').data('path') + "libs/smarteditor/SmartEditor2Skin.html",
			fCreator: "createSEditor2"
		});
	</script>
</html>
```





이제 브라우저에서 http://localhost:8080/smarteditor 로 접속해보자. 아래와 같이 나오면 성공이다.

![add-smarteditor-6](\assets\images\add-smarteditor\add-smarteditor-6.JPG)





## 에디터에 작성한 내용 JS에서 불러오기

에디터를 열심히 만들어놓고 정작 내부의 내용을 가져오지 못하면 아무 소용이 없다. 내부 내용을 가져오는 코드를 작성해보자. 일단 JSP 파일 먼저 아래처럼 수정한다.

```jsp
<%@ page language="java" contentType="text/html; charset=EUC-KR" pageEncoding="EUC-KR"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>

<!DOCTYPE html>
<html>
	<head>
		<meta charset="EUC-KR">
		<link id="contextPath" data-path="<c:url value='/'/>" />
		<link rel="stylesheet" href="<c:url value='/libs/bootstrap-4.4.1-dist/css/bootstrap.min.css'/>">
		
		<script src="<c:url value='/libs/jquery-3.4.1.min.js'/>"></script>
		<script src="<c:url value='/libs/bootstrap-4.4.1-dist/js/bootstrap.min.js'/>"></script>
		<script type="text/javascript" src="<c:url value='/libs/smarteditor/js/service/HuskyEZCreator.js'/>" charset="utf-8"></script>
		
		<title>Insert title here</title>
	</head>
	<body>
		<textarea name="ir1" id="ir1" rows="10" cols="100">에디터에 기본으로 삽입할 글(수정 모드)이 없다면 이 value 값을 지정하지 않으시면 됩니다.</textarea>
		<button type="button" class="btn">작성내용 콘솔에 찍기!!</button>
	</body>
	
	<script>
		var oEditors = [];
		nhn.husky.EZCreator.createInIFrame({
			oAppRef: oEditors,
			elPlaceHolder: "ir1",
			sSkinURI: $('#contextPath').data('path') + "libs/smarteditor/SmartEditor2Skin.html",
			fCreator: "createSEditor2"
		});

		$(".btn").on("click", function() {
			oEditors.getById["ir1"].exec("UPDATE_CONTENTS_FIELD", []);
			var value = document.getElementById("ir1").value;
			console.log(value);
		});
	</script>
</html>
```





버튼을 하나 추가했고, 해당 버튼에 이벤트를 하나 추가했다. 이제 에디터에 아무 내용을 작성한 뒤 개발자 도구의 콘솔창을 한번 봐보자.

![add-smarteditor-7](\assets\images\add-smarteditor\add-smarteditor-7.JPG)

콘솔에 HTML 구조로 찍힌다면 성공했다.

추가로 이미지를 추가하는 내용을 작성하려고 했는데, 네이버측에서 이미지를 추가하는 예제가 취약점이 발생되어서 최신 버전에는 해당 예시를 없앤 뒤 배포중이다. 이전 버전을 다운받으면 해당 예시를 볼 수 있는데 PHP, Jindo 기반으로 되어 있어서 이를 JSP, JAVA, jQuery 기반으로 적용해보다가 아직 적용을 하지 못해서 작성하지 못했다..

추후 커스터마이징을 완료하고 이미지 추가가 정상작동된다면 포스트를 추가할 예정이다.