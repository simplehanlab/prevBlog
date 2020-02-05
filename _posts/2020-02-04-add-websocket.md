---
title: "Spring 프로젝트에 WebSocket 적용하기"
comments: true
categories:
  - Spring
tags:
  - Spring
  - STS
  - JSP
  - WebSocket
date: "2020-02-04 16:00"
---

웹 프로젝트를 진행하다보면 단순히 Restful API로 처리할 수 없는 요건이 생길 수 있다. 보통 HTTP 통신은 클라이언트에서 Request를 날린 후 서버의 Response를 받는 형식인데, Server에서부터 통신이 시작되야되는 요건이 있을 수 있다. 혹은 지속적으로 connection을 맺고 있어야 한다거나...*(실시간 채팅 혹은 알림 서비스가 그런 요건에 속한다.)*

이를 구현할 때는 HTTP 프로토콜을 이용하여 Polling 방식을 통해 구현할 수도 있지만, 이런 요구조건을 맞추기 위해 WebSocket API가 등장했다. 이름 그대로 Web 상에서 Socket 방식으로 통신을 구현한다.

WebSocket은 구버전 브라우저에선 지원하지 않는 다는 단점이 있는데, 이를 보완하기 위해 SockJS라는 모듈이 있다. SockJS는 구버전 브라우저에서도 WebSocket 코드가 그대로 동작하도록 도와준다. (polyfill로 볼 수 있을 듯 하다.)

본 포스트는 Spring에서 제공하는 가이드를 보고 만들었기 때문에 자세한 사항은 아래 링크 가이드에서 볼 수 있다.

[Spring WebSocket 적용 가이드](https://spring.io/guides/gs/messaging-stomp-websocket/)



또한 이전에 진행한 Bootstrap을 적용한 Spring 프로젝트를 이용해서 진행한다.

[Spring 프로젝트에 Bootstrap 적용하기](/spring/add-bootstrap)



## WebSocket 모듈 적용

프로젝트에 WebSocket 모듈을 적용해보자. pom.xml에 아래 코드를 통해 모듈을 추가하자.

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-websocket</artifactId>
</dependency>

<!-- websocket -->
<dependency>
    <groupId>org.webjars</groupId>
    <artifactId>webjars-locator-core</artifactId>
</dependency>
<dependency>
    <groupId>org.webjars</groupId>
    <artifactId>sockjs-client</artifactId>
    <version>1.0.2</version>
</dependency>
<dependency>
    <groupId>org.webjars</groupId>
    <artifactId>stomp-websocket</artifactId>
    <version>2.3.3</version>
</dependency>
```



이번 포스트에서는 가이드대로 webjars 기술을 이용해보았다. webjars는 간단히 말하면 웹 라이브러리 의존성 관리 도구이다. 웹 라이브러리를 JAR 파일 안에 패키징해준다. 그렇기 때문에 따로 라이브러리를 다운로드 받을 필요 없이 /webjars path를 통해서 라이브러리를 사용할 수 있게 해준다.



## JAVA 코드 생성

그 다음 두가지 객체를 만들어 준다. 하나는 HelloMessage.java 파일로 클라이언트에서 입력한 이름을 받는 객체이다. 나머지 하나는 Greeting.java 파일로 받은 이름과 같이 클라이언트쪽으로 응답할 때 사용하는 객체이다. 일단 com.example.demo.websocket.vo 패키지를 만든 뒤 아래 코드와 같이 객체를 추가해주자.

![add-websocket-1](\assets\images\add-websocket\add-websocket-1.JPG)

```java
package com.example.demo.websocket.vo;

public class HelloMessage {

	private String name;

	public HelloMessage() {}

	public HelloMessage(String name) {
		this.name = name;
	}

	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}
}
```

```java
package com.example.demo.websocket.vo;

public class Greeting {

	private String content;

	public Greeting() {}

	public Greeting(String content) {
		this.content = content;
	}

	public String getContent() {
		return content;
	}
}
```



그 다음 com.example.demo.websocket.web 패키지를 만든 뒤 GreetingController.java 파일을 추가하자. 코드는 아래와 같다.

![add-websocket-2](\assets\images\add-websocket\add-websocket-2.JPG)

```java
package com.example.demo.websocket.web;

import org.springframework.messaging.handler.annotation.MessageMapping;
import org.springframework.messaging.handler.annotation.SendTo;
import org.springframework.stereotype.Controller;
import org.springframework.web.util.HtmlUtils;

import com.example.demo.websocket.vo.Greeting;
import com.example.demo.websocket.vo.HelloMessage;

@Controller
public class GreetingController {
	@MessageMapping("/hello")	// 요청을 받는 곳
	@SendTo("/topic/greetings")	// 괄호 안의 메시지 브로커에 return 값을 보낸다.
	public Greeting greeting(HelloMessage message) throws Exception {
		Thread.sleep(1000);		// 수동 딜레이
		return new Greeting("Hello, " + HtmlUtils.htmlEscape(message.getName()) + "!");
	}
    
    @RequestMapping("/websocket")
	public String websocket() {
		return "websocket";
	}
}
```



이제 JSP에서 테스트 코드를 작성하기 전에, WebSocket 설정 파일을 작성하자. com.example.demo.config 패키지를 만들고 WebSocketConfig.java 파일을 작성하자. 코드는 아래와 같다.

![add-websocket-3](\assets\images\add-websocket\add-websocket-3.JPG)

```java
package com.example.demo.config;

import org.springframework.context.annotation.Configuration;
import org.springframework.messaging.simp.config.MessageBrokerRegistry;
import org.springframework.web.socket.config.annotation.EnableWebSocketMessageBroker;
import org.springframework.web.socket.config.annotation.StompEndpointRegistry;
import org.springframework.web.socket.config.annotation.WebSocketMessageBrokerConfigurer;

@Configuration
@EnableWebSocketMessageBroker
public class WebSocketConfig implements WebSocketMessageBrokerConfigurer {
	@Override
	public void configureMessageBroker(MessageBrokerRegistry config) {
		config.enableSimpleBroker("/topic");	// /topic path에 메시지 브로커 생성
		config.setApplicationDestinationPrefixes("/app");	// 요청받는 path의 앞단
	}
	
	@Override
	public void registerStompEndpoints(StompEndpointRegistry registry) {
		registry.addEndpoint("/gs-guide-websocket").withSockJS();	// WebSocket 기본 path
	}
}
```



## JSP, JS 코드 생성

이제 JSP 파일을 추가해보자. JSP 파일은 src/main/webapp/WEB-INF/jsp 폴더 안에 websocket.jsp로 만들고 코드는 아래와 같이 작성했다.

![add-websocket-4](\assets\images\add-websocket\add-websocket-4.JPG)

```jsp
<%@ page language="java" contentType="text/html; charset=EUC-KR" pageEncoding="EUC-KR"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>

<!DOCTYPE html>
<html>
	<head>
		<meta charset="EUC-KR">
		<link rel="stylesheet" href="<c:url value='/libs/bootstrap-4.4.1-dist/css/bootstrap.min.css'/>">
		<script src="/webjars/sockjs-client/sockjs.min.js"></script>
		<script src="/webjars/stomp-websocket/stomp.min.js"></script>
		<script src="<c:url value='/libs/jquery-3.4.1.min.js'/>"></script>
		<script src="<c:url value='/libs/bootstrap-4.4.1-dist/js/bootstrap.min.js'/>"></script>
		<title>Insert title here</title>
	</head>
	<body>
		<section id="main-content">
			<section class="wrapper">
				<h3><i class="fa fa-angle-right"></i> WebSocket</h3>
				<div class="row">
				    <div class="col-md-6">
				        <form class="form-inline">
				            <div class="form-group">
								<label for="connect">WebSocket connection:</label>
				                <button id="connect" class="btn btn-default" type="submit">Connect</button>
				                <button id="disconnect" class="btn btn-default" type="submit" disabled="disabled">Disconnect
				                </button>
				            </div>
				        </form>
				    </div>
				    <div class="col-md-6">
				        <form class="form-inline">
				            <div class="form-group">
				                <label for="name">What is your name?</label>
				                <input type="text" id="name" class="form-control" placeholder="Your name here...">
				            </div>
				            <button id="send" class="btn btn-default" type="submit">Send</button>
				        </form>
				    </div>
				</div>
				<div class="row">
				    <div class="col-md-12">
				        <table id="conversation" class="table table-striped">
				            <thead>
				            <tr>
				                <th>Greetings</th>
				            </tr>
				            </thead>
				            <tbody id="greetings">
				            </tbody>
				        </table>
				    </div>
				</div>
			</section>
		</section>
	</body>
</html>
```



브라우저에서 키보드의 **F12**키를 눌러서 개발자 도구를 연 뒤 **Network**탭을 클릭하자. 그 후 브라우저를 새로고침한 뒤 sockjs 모듈과 stomp 모듈이 제대로 호출되는지 확인해보자. webjars를 이용하여 라이브러리를 직접 추가해주지 않아도 호출되는 모습을 확인할 수 있다.

![add-websocket-5](\assets\images\add-websocket\add-websocket-5.JPG)



마지막으로 websocket.js 파일을 추가해주려 한다. src/resources/static/js/websocket 폴더 내에 websocket.js 파일을 추가해주자.

![add-websocket-6](\assets\images\add-websocket\add-websocket-6.JPG)



그 다음 아래 코드를 입력하자.

```javascript
var stompClient = null;

// connected 상태 변경
function setConnected(connected) {
    $("#connect").prop("disabled", connected);
    $("#disconnect").prop("disabled", !connected);
    if (connected) {
        $("#conversation").show();
    }
    else {
        $("#conversation").hide();
    }
    $("#greetings").html("");
}

// WebSocket 접속
function connect() {
    var socket = new SockJS('/gs-guide-websocket');	// WebSocketConfig.java에서 설정한 endpoint
    stompClient = Stomp.over(socket);	// socket을 이용하여 stompClient 생성
    // socket 접속
    stompClient.connect({}, function (frame) {
        setConnected(true);
        console.log('Connected: ' + frame);
        // '/topic/greetings' 메시지 브로커 구독
        // 해당 토픽에 메시지가 게시되면 이 클라이언트가 메시지를 읽어서 클라이언트의 콜백함수를 통해 메시지 전달
        stompClient.subscribe('/topic/greetings', function (greeting) {
            showGreeting(JSON.parse(greeting.body).content);
        });
    });
}

// WebSocket 접속 끊기
function disconnect() {
    if (stompClient !== null) {
        stompClient.disconnect();
    }
    setConnected(false);
    console.log("Disconnected");
}

// message 보내기
function sendName() {
    stompClient.send("/app/hello", {}, JSON.stringify({'name': $("#name").val()}));
}

// 서버로부터 받은 메시지 출력
function showGreeting(message) {
    $("#greetings").append("<tr><td>" + message + "</td></tr>");
}

$(function () {
    $("form").on('submit', function (e) {
        e.preventDefault();
    });
    $( "#connect" ).click(function() { connect(); });
    $( "#disconnect" ).click(function() { disconnect(); });
    $( "#send" ).click(function() { sendName(); });
});
```



websocket.jsp 파일에 아래 코드를 추가해준다.

```jsp
...
<script src="<c:url value='/js/websocket/websocket.js'/>"></script>
...
```



## 테스트해보기

이제 브라우저를 새로고침하여 개발자도구 콘솔에서 에러가 없는지 확인 후, **Connect** 버튼을 눌러보자. Connect 버튼을 누른 뒤에 개발자 도구의 콘솔에 아래 이미지와 같이 나타나면 성공이다.

![add-websocket-7](\assets\images\add-websocket\add-websocket-7.JPG)



이제 Connected 상태에서 오른쪽 **What is your name?**이라 써있는 간단한 폼에 글자를 입력한 뒤 **Send**버튼을 클릭해보자. 정상작동됬다면 아래 이미지처럼 뜰 것이다.

![add-websocket-8](\assets\images\add-websocket\add-websocket-8.JPG)



마지막으로 **Disconnect** 버튼을 클릭하여 연결을 끊어보자. 이 역시 정상작동하면 아래 이미지처럼 나올 것이다.

![add-websocket-9](\assets\images\add-websocket\add-websocket-9.JPG)



## JAVA에서 어노테이션 외에 코드상으로 WebSocket 사용하기

위에서 WebSocket의 동작을 확인해봤다. 하지만 클라이언트에서 요청을 하면 해당 요청을 매핑하여 받은 뒤 브로커로 보내는 방식이 얼핏보면 Restful API와 별다를 바가 없어 보인다. 물론 내부적으로는 socket을 통해서 메시지 브로커가 토픽상의 메시지를 주고 받는 것이지만..

이런 식의 구현이 아닌, JAVA에서 무언가 일을 처리한 뒤 메시지 브로커로 보내는 방법은 없을까 싶어서 찾아보니 코드상으로도 메시지 브로커에 메시지를 보내는 방법이 있었다. 단순히 웹 상에서 요청을 받았을 때 메시지 브로커로 데이터를 보내는 것이 아니라 서버가 구동되는 순간이라던지, 스케쥴러가 돌아간 순간이라던지 순수 JAVA 코드가 실행된 후에 메시지 브로커로 데이터를 보낼 수 있는 방법이다.

하지만 본 포스트에 사용된 프로젝트로는 마땅한 예제를 만들기엔 조금 힘들어서 예제 코드로만 설명을 하려 한다.

일단 com.example.demo.websocket.service 패키지를 만든 뒤 WebSocketService.java 파일을 만들자.

![add-websocket-10](\assets\images\add-websocket\add-websocket-10.JPG)



코드는 아래처럼 추가해주자.

```java
package com.example.demo.websocket.service;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.messaging.simp.SimpMessagingTemplate;
import org.springframework.stereotype.Service;

import com.example.demo.websocket.vo.Greeting;

@Service
public class WebSocketService {
	
	@Autowired
	private SimpMessagingTemplate template;
	
	public void myTestService() {
		// '/topic/greetings' 토픽에 메시지 게시
		template.convertAndSend("/topic/greetings", new Greeting("Hello"));
	}
}
```



이제 WebSocketService의 myTestService 메소드를 호출하면 */topic/greetings*토픽에 두번째 파라미터를 데이터로 게시하게 된다. 이는 필요한 부분에 커스터마이징하여 사용하면 될 것으로 보인다.