---
title: "Spring 프로젝트에 Bootstrap 적용하기"
comments: true
categories:
  - Spring
tags:
  - Spring
  - STS
  - JSP
  - Bootstrap
date: "2020-02-04 15:00"
---

프론트엔드를 개발하다보면 UI 개발도 같이 진행이 되는데, 이를 개발자가 직접 CSS를 작성하면서 개발하게 되면 개발 기간이 많이 늘어날 수 밖에 없다. 이럴 때 미리 개발되어 있는 UI 프레임워크 등을 가져다 쓰면 편리하지 않을까?

다행히 프론트엔드 프레임워크 중에 Bootstrap이라는 프레임워크가 있는데 이 프레임워크를 이용해서 UI 단 개발의 공수를 많이 줄일 수 있다. 이제부터 이 Bootstrap을 저번에 만든 Spring 기본 프로젝트에 적용하여 사용해보자.

저번에 만든 Spring 기본 프로젝트 관련 포스트는 아래 링크에서 확인할 수 있다.

[Spring 프레임워크를 이용한 프로젝트 생성 (with STS, JSP)](/spring/start-spring)



## Bootstrap 다운받기 및 적용

Bootstrap을 적용하는 방법엔 크게 3가지가 있다. 리소스를 직접 다운받아서 내 프로젝트에 적용하는 방법과 CDN 서비스를 이용하여 네트워크 상에서 바로 받아서 사용하는 방법, 마지막으로 패키지 매니저(npm, yarn 등등)를 이용하여 받는 방법이다.

1. CDN을 사용하게 되면 다운로드 받고 내 프로젝트에 적용하는 번거로운 작업 없이 바로 Bootstrap을 사용할 수 있지만, 해당 CDN 서버가 다운되거나 뭔가 문제가 생기면 내 프로젝트에도 영향이 간다는 단점이 있다.
2. 안정성을 높이려면 리소스를 직접 다운로드 받아서 사용하는게 좋겠지만 이 방법은 버전 관리가 귀찮다는 단점이 있다.
3. 패키지 매니저를 이용한 방법은 버전 관리도 직접 다운로드보다 수월하고, 리소스 자체도 내려받기 때문에 패키지 매니저를 통해 다운로드 받는 순간만 패키지 매니저 서버가 살아있으면 리소스는 내 프로젝트 내부에서 사용할 수 있다. 하지만 이용하려는 프로젝트 환경 상 패키지 매니저를 지원하지 않는다면 해당 환경을 잡는데 좀 힘들 수 있다.

본 포스트에서는 두가지 방법을 소개 해보려 하는데, 직접 다운로드 받는 방법과 CDN 서비스를 이용하는 방법이다. CDN을 이용한 적용은 JSP 파일 내부의 css, js의 링크 값만 바꿔주면 되기 때문에 중간에 같이 소개하려 한다.

일단 [Bootstrap 다운로드 사이트](https://getbootstrap.com/docs/4.4/getting-started/download/)에 접속하여 Bootstrap 리소스를 다운받자.

![add-bootstrap-1](\assets\images\add-bootstrap\add-bootstrap-1.JPG)



다운로드 받은 리소스를 내 프로젝트의 src/main/resources/static/libs 폴더 안에 넣어주자.

![add-bootstrap-2](\assets\images\add-bootstrap\add-bootstrap-2.JPG)



추가로 bootstrap 사용에 jQuery가 필요하므로 [jQuery 다운로드 사이트](https://jquery.com/download/)에서 jQuery도 다운받아주자. *(jQuery 역시 CDN, 패키지 매니저를 통해 적용할 수 있다.)*

![add-bootstrap-3](\assets\images\add-bootstrap\add-bootstrap-3.JPG)



필요한 모듈을 다 받았다면 JSP 파일에 아래 코드를 추가하여 모듈을 적용해주자.

```jsp
<%@ page language="java" contentType="text/html; charset=EUC-KR" pageEncoding="EUC-KR"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>

<!DOCTYPE html>
<html>
    <head>
        <meta charset="EUC-KR">
        <link rel="stylesheet" href="<c:url value='/libs/bootstrap-4.4.1-dist/css/bootstrap.min.css'/>">
        <title>Insert title here</title>
	</head>
	<body>
		Hello world!<br/>
		My name is ${person.name}<br/>
		I'm ${person.age} years old<br/>
	</body>
	
	<script src="<c:url value='/libs/jquery-3.4.1.min.js'/>"></script>
	<script src="<c:url value='/libs/bootstrap-4.4.1-dist/js/bootstrap.min.js'/>"></script>
</html>
```



CDN 서비스를 이용하려면 bootstrap, jQuery를 다운로드 받을 필요 없이 아래와 같이 코드를 적용하면 된다.

```jsp
<%@ page language="java" contentType="text/html; charset=EUC-KR" pageEncoding="EUC-KR"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>

<!DOCTYPE html>
<html>
	<head>
		<meta charset="EUC-KR">
		<link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.4.1/css/bootstrap.min.css" integrity="sha384-Vkoo8x4CGsO3+Hhxv8T/Q5PaXtkKtu6ug5TOeNV6gBiFeWPGFN9MuhOf23Q9Ifjh" crossorigin="anonymous">
		<title>Insert title here</title>
	</head>
	<body>
		Hello world!<br/>
		My name is ${person.name}<br/>
		I'm ${person.age} years old<br/>
	</body>
	
	<script src="https://code.jquery.com/jquery-3.4.1.slim.min.js" integrity="sha384-J6qa4849blE2+poT4WnyKhv5vZF5SrPo0iEjwBvKU7imGFAV0wwj1yYfoRSJoZ+n" crossorigin="anonymous"></script>
    <script src="https://cdn.jsdelivr.net/npm/popper.js@1.16.0/dist/umd/popper.min.js" integrity="sha384-Q6E9RHvbIyZFJoft+2mJbHaEWldlvI9IOYy5n3zV9zzTtmI3UksdQRVvoxMfooAo" crossorigin="anonymous"></script>
    <script src="https://stackpath.bootstrapcdn.com/bootstrap/4.4.1/js/bootstrap.min.js" integrity="sha384-wfSDF2E50Y2D1uUdj0O3uMBJnjuUD4Ih7YwaYd1iqfktj0Uod8GCExl3Og8ifwB6" crossorigin="anonymous"></script>
</html>
```



코드 적용 후 브라우저에서 키보드의**F12**키를 눌러서 개발자 도구를 켠 뒤에 **Network**탭을 클릭한 뒤 브라우저를 새로고침 해보자. 그 후 아래처럼 리소스를 에러없이 불러오면 성공이다.

![add-bootstrap-4](\assets\images\add-bootstrap\add-bootstrap-4.JPG)



그 다음 JSP 파일을 아래 코드와 같이 수정한 뒤 브라우저를 새로고침하여 bootstrap이 잘 적용되었는지 확인해보자.

```jsp
<%@ page language="java" contentType="text/html; charset=EUC-KR" pageEncoding="EUC-KR"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>

<!DOCTYPE html>
<html>
	<head>
		<meta charset="EUC-KR">
		<link rel="stylesheet" href="<c:url value='/libs/bootstrap-4.4.1-dist/css/bootstrap.min.css'/>">
		<title>Insert title here</title>
	</head>
	<body>
		<!-- Navigation -->
		<nav class="navbar navbar-expand-lg navbar-dark bg-dark static-top">
		  <div class="container">
		    <a class="navbar-brand" href="#">
		          <img src="http://placehold.it/150x50?text=Logo" alt="">
		        </a>
		    <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarResponsive" aria-controls="navbarResponsive" aria-expanded="false" aria-label="Toggle navigation">
		          <span class="navbar-toggler-icon"></span>
		        </button>
		    <div class="collapse navbar-collapse" id="navbarResponsive">
		      <ul class="navbar-nav ml-auto">
		        <li class="nav-item active">
		          <a class="nav-link" href="#">Home
		                <span class="sr-only">(current)</span>
		              </a>
		        </li>
		        <li class="nav-item">
		          <a class="nav-link" href="#">About</a>
		        </li>
		        <li class="nav-item">
		          <a class="nav-link" href="#">Services</a>
		        </li>
		        <li class="nav-item">
		          <a class="nav-link" href="#">Contact</a>
		        </li>
		      </ul>
		    </div>
		  </div>
		</nav>
		
		<!-- Page Content -->
		<div class="container">
		  <h1 class="mt-4">Logo Nav by Start Bootstrap</h1>
		  <p>The logo in the navbar is now a default Bootstrap feature in Bootstrap 4! Make sure to set the width and height of the logo within the HTML or with CSS. For best results, use an SVG image as your logo.</p>
		</div>
		<!-- /.container -->
	</body>
	
	<script src="<c:url value='/libs/jquery-3.4.1.min.js'/>"></script>
	<script src="<c:url value='/libs/bootstrap-4.4.1-dist/js/bootstrap.min.js'/>"></script>
</html>
```

![add-bootstrap-5](\assets\images\add-bootstrap\add-bootstrap-5.JPG)