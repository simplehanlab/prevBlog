---
title: "Spring 프레임워크를 이용한 프로젝트 생성 (with STS, JSP)"
comments: true
categories:
  - Spring
tags:
  - Spring
  - STS
  - JSP
date: "2020-02-04 14:00"
---

## STS (Spring Tool Suite) 받기

Spring 프레임워크를 이용하여 프로젝트를 만들기위해 IDE를 정해야하는데, 여러가지 IDE가 있겠지만 Spring 프로젝트 제작시에는 STS를 이용하는게 좋은 방법이라 생각된다. 일단 이클립스 기반이기 때문에 이클립스로 JAVA 프로젝트를 개발하던 개발자들도 익숙하게 사용이 가능하고, 무료로 이용할 수 있다. Spring 팀에서 제작했기 때문에 Spring 프레임워크와 호환도 좋다. 이름부터가 Spring Tool Suite 이니..

그 외에 Spring 팀에서 제공하는 IDE가 더 있는데 Visual Studio Code 기반인 IDE와 Theia 기반인 IDE가 있다. 하지만 본 포스트에서는 STS만 다룬다.

STS를 받기 위해선 STS 사이트에 접속해야한다. 아래 주소를 통해 접속할 수 있다.

[Spring Tools](https://spring.io/tools)

STS 사이트에 접속하면 아래 사진과 같이 IDE 다운로드 링크가 있으니 필요한 걸 받으면 된다. 본 포스트는 Windows 버전으로 진행한다. 버전은 4.5.1.RELEASE 버전으로 진행한다.

![start-spring-1](\assets\images\start-spring\start-spring-1.JPG)

Windows 버전을 다운받으면 jar 파일을 내려받게 된다. 이 jar파일의 압축을 풀면 **contents** 폴더가 나오게 되는데 이 안에 STS가 있다.



## Spring 프로젝트 생성

다운받은 STS를 실행하면 아래와 같이 뜬다.

![start-spring-2](\assets\images\start-spring\start-spring-2.JPG)

좌측 Package Explorer 창에서 **Create new Spring Starter Project**를 클릭하면 Spring 프로젝트의 생성을 도와주는 프롬프트가 나오게 된다.



![start-spring-3](\assets\images\start-spring\start-spring-3.JPG)

[https://start.spring.io](https://start.spring.io) 에서 프로젝트 생성에 필요한 정보를 받아올 수 있다. Spring 프로젝트는 Web상에서 생성할 수도 있는데 해당 링크에 들어가서 필요한 정보를 입력, 체크한 뒤 Generate를 하면 프로젝트를 내려받을 수가 있다. 궁금하면 링크에 접속해보자.

필요한 정보를 입력한 뒤 Next를 클릭하자.



![start-spring-4](\assets\images\start-spring\start-spring-4.JPG)

Next를 입력하면 Spring에서 제공하는 서비스 모듈을 선택하여 프로젝트 생성 시 같이 포함하여 생성할 수 있다. 여기서 필요한 모듈을 선택하면 되는데, 본 포스트에서는 아래와 같이 Web의 Spring Web 항목만 체크하여 만들었다.



![start-spring-5](\assets\images\start-spring\start-spring-5.JPG)

필요한 모듈을 모두 체크하였으면 **Finish**버튼을 클릭하여 프로젝트를 생성한다.



![start-spring-6](\assets\images\start-spring\start-spring-6.JPG)

프로젝트가 생성되면 구조는 위의 이미지와 같다.

```bash
my-project
├── src
|  ├── main
|  |  ├── java                 # JAVA 파일
|  |  └── resources            # 정적 파일 (css, js, image, properties 등등)
|  └── test
|     └── java                 # 테스트 용 JAVA 파일 (JUnit 등)
├── JRE System Library         # JRE
├── Maven Dependencies         # Maven 의존 모듈
├── target                     # Build 결과물 보관 장소
├── HELP.md                    # Spring 프로젝트 도움말 및 가이드
├── mvnw                       # Maven Wrapper 스크립트
├── mvnw.cmd                   # Maven Wrapper 스크립트의 배치 버전
└── pom.xml                    # Maven 빌드 옵션 설정 파일
```

*(위의 구조는 폴더 구조가 아닌 Package Explorer에서 표출해주는 구조이다.)*



## 웹페이지 생성 및 접속해보기

예전에 이클립스, 전자정부 프레임워크, 스프링 프레임워크를 이용하여 개발할때 서버를 띄우기 위해서 톰캣을 같이 설치하여 톰캣으로 서버를 띄워줘야 했으나, 요즘 스프링 프레임워크는 스프링부트의 내장톰캣을 이용하여 서버를 띄울 수 있기 때문에 별도로 톰캣을 설치할 필요가 없다.

*(운영환경이라면 톰캣의 커스터마이징이 필요할 수 있기 때문에 톰캣 설치를 추천한다.)*

다만 스프링부트는 기본적으로 JSP를 지원하지 않기 때문에 JSP를 사용하기 위해선 별도로 메이븐을 통해 모듈을 받아야 한다. 당장에 필요한 모듈은 JSP를 구동시켜주는 내장톰캣 모듈과 JSTL을 지원해주는 JSTL 모듈이기 때문에 해당 모듈들을 **pom.xml**에 지정해주자.

```xml
<dependency>
    <groupId>javax.servlet</groupId>
    <artifactId>jstl</artifactId>
</dependency>

<dependency>
    <groupId>org.apache.tomcat.embed</groupId>
    <artifactId>tomcat-embed-jasper</artifactId>
</dependency>
```



모듈 추가가 완료됬다면 다음으로 java 파일을 추가해보자. MVC 패턴을 적용하여 페이지를 만들 예정이다. 그렇기 때문에 일단 Controller를 만들어준다. 기본 생성된 프로젝트는 아래 이미지와 같이 스프링부트 기본 java 파일만 있다.

![start-spring-7](\assets\images\start-spring\start-spring-7.JPG)



여기에 패키지를 추가하고 Controller를 추가해보자. 패키지는 com.example.demo.main으로 만들고, 그 아래에 com.example.demo.main.controller 패키지를 하나 더 추가한 뒤 해당 패키지에 controller를 만든다. 아래 이미지를 보자. *(controller 패키지는 web 패키지로 사용하는 경우도 있다. // com.example.demo.main.web)*

![start-spring-8](\assets\images\start-spring\start-spring-8.JPG)



MainController.java는 아직 아무 내용도 없다. 이제 이 java 파일이 controller라는 명시를 어노테이션을 통해서 지정해준 뒤, 루트(/) 주소에 View를 지정하자.

```java
package com.example.demo.main.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;

@Controller		// Controller 어노테이션을 통해 이제부터 이 java 파일은 Controller 역할을 수행한다.
public class MainController {
	@RequestMapping(value="/")		// 컨트롤 할 URL을 지정해준다. URL의 path 부분을 작성해주면 된다.
	public String main() {
		return "main";		// View의 폴더, 파일 이름을 문자열로 리턴해준다.
	}
}
```



주목할 부분은 **@Controller**, **@RequestMapping** 어노테이션들과 **return 값의 문자열**이다. 최신 스프링 프레임워크에서 Controller를 사용하고 싶다면 간단하게 @Controller 어노테이션을 class 위에 명시해주기만 하면 된다. 그리고 URL 라우팅은 메소드 위에 @RequestMapping 어노테이션을 통해 간단히 구현할 수 있다. *(class 위에 @RequestMapping 어노테이션을 명시하여 해당 Controller에 전체적으로 공통 Path를 지정해주는 방법도 있다.)*

그리고 마지막으로 주목해야할 return 문자열은 View의 폴더, 이름등을 지정해주는데 그렇다면 이 View는 어느 위치에 추가하는 것이며, 파일의 기본 폴더나 확장자 등은 스프링에서 자동으로 처리해 주는 것일까?

어느정도 자동으로 처리해주는 부분이 있지만, 기본 폴더나 확장자 등 prefix와 suffix는 따로 잡아줘야 한다. 이를 application.properties 라는 설정 파일에서 잡아줄 수 있다. 이 파일은 아래 이미지와 같이 스프링 프로젝트 생성 시 src/main/resources 폴더 안에 자동으로 생성된다.

![start-spring-9](\assets\images\start-spring\start-spring-9.JPG)



이제 이 properties 파일을 열고 아래 두 줄을 추가한다.

```properties
spring.mvc.view.prefix=/WEB-INF/jsp/
spring.mvc.view.suffix=.jsp
```



prefix는 controller에서 return된 문자열의 앞에 붙는 문자이고, suffix는 뒤에 붙는 문자이다. 위의 controller에서 main을 리턴하면 최종적으로 **/WEB-INF/jsp/main.jsp** 문자열이 완성되는 것이다.

하지만 프로젝트 어딜 찾아봐도 WEB-INF 폴더는 보이지 않는데, 이 폴더는 직접 만들어줘야 한다. 그 경로는 src/main/webapp 의 아래인데, main 폴더를 보면 webapp 폴더도 보이지 않는다. 그래서 webapp 폴더도 직접 만들어줘야 한다. *(위에서 View의 폴더 지정 등을 어느정도 자동으로 처리해주는 부분이 있다고 했는데 이 webapp 폴더가 그것이다. 기본적으로 내장 톰캣을 사용한 환경이기 때문에 webapp 폴더를 기본 폴더로 간주하고, 이를 prefix 등에 지정할 필요가 없다.)*

properties 파일에 위 두줄을 추가했으면 이제 jsp 파일을 추가해주자. 아래 이미지처럼 추가해주면 된다.

![start-spring-10](\assets\images\start-spring\start-spring-10.JPG)



이 jsp 파일의 body안에 **Hello world!** 라고 작성해보자. 아래 코드를 참조하자.

```jsp
<%@ page language="java" contentType="text/html; charset=EUC-KR" pageEncoding="EUC-KR"%>

<!DOCTYPE html>
<html>
<head>
<meta charset="EUC-KR">
<title>Insert title here</title>
</head>
<body>
	Hello world!
</body>
</html>
```



이제 이 페이지를 브라우저에서 띄워보자. STS를 따로 건드리지 않았다면 좌측 하단에 **Boot Dashboard**가 있다. *(만약 없다면 위에 Window > Show View > Other를 클릭하여 Boot Dashboard를 누르면 된다.)*

이 Boot Dashboard를 보면 local 항목이 있을텐데 그 안에 내 프로젝트가 있다. 내 프로젝트를 선택한 뒤 그 위의 실행 혹은 디버그 버튼을 클릭하여 프로젝트를 실행하자. 아래 이미지를 참조

![start-spring-11](\assets\images\start-spring\start-spring-11.JPG)



실행이 완료되면 몇번 포트로 실행이 됬는지 콘솔에 출력된다. 아래 이미지 참조

![start-spring-12](\assets\images\start-spring\start-spring-12.JPG)



포트를 지정하지 않았다면 기본으로 8080포트에 서버가 뜨게 된다. *(포트 변경은 본 포스트 아래쪽 참조)*

이제 사용하는 브라우저에서 http://localhost:8080에 접속해보자. 아래 이미지처럼 Hello world!가 화면에 찍히면 성공!

![start-spring-13](\assets\images\start-spring\start-spring-13.JPG)



## 간단하게 Model 추가해보기

이제 기본 main.jsp 페이지에서 이름과 나이를 출력하는 간단한 model을 만들어보자. com.example.demo.main.vo 패키지를 추가한 뒤 Person.java 파일을 추가해주자. *(vo 패키지는 dto 등으로도 쓸 수 있다. // 더 세세하게 들어가면 dto와 vo의 차이점이 있지만 본 포스팅에서는 그렇게까지 나누지 않을 예정이고 그냥 편의상 vo로 사용)*

![start-spring-14](\assets\images\start-spring\start-spring-14.JPG)



Person.java 안의 내용은 아래처럼 작성해주자.

```java
package com.example.demo.main.vo;

public class Person {
	private String name = "Simple";
	private int age = 20;
	
	public String getName() {
		return name;
	}
	
	public int getAge() {
		return age;
	}
}
```



이제 jsp에서 name과 age를 출력해보자.

***(지금은 간단한 구현을 위해 속성에 값을 직접 할당했지만 실 개발에선  값을 직접 할당하지 않고 거의 대부분의 경우에 Setter를 통해 값을 지정하고 조회한다. 추가로 Lombok등을 이용하여 Getter, Setter의 코드를 줄이는 등의 방법도 있다.)***



다시 MainController.java 파일을 열어서 아래와 같이 코드를 수정해주자.

```java
package com.example.demo.main.controller;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.RequestMapping;

import com.example.demo.main.vo.Person;

// Controller 어노테이션을 통해 이제부터 이 java 파일은 Controller 역할을 수행한다.
@Controller
public class MainController {
	@RequestMapping(value="/")		// URL의 path 부분을 작성해주면 된다.
	public String main(Model model) {
		model.addAttribute("person", new Person());		// 모델 부여
		return "main";		// View의 폴더, 파일 이름을 문자열로 리턴해준다.
	}
}
```



그 다음 main.jsp 파일을 열어서 아래와 같이 수정해주자.

```jsp
<%@ page language="java" contentType="text/html; charset=EUC-KR" pageEncoding="EUC-KR"%>

<!DOCTYPE html>
<html>
<head>
<meta charset="EUC-KR">
<title>Insert title here</title>
</head>
<body>
	Hello world!<br/>
	My name is ${person.name}<br/>
	I'm ${person.age} years old<br/>
</body>
</html>
```



그 다음 서버를 재시작하여 브라우저에 아래와 같이 잘 나오는 지 확인해보자.

![start-spring-15](\assets\images\start-spring\start-spring-15.JPG)



## RestController 사용해보기

요즘은 HTTP 프로토콜을 이용한 API 사용에 Restful API를 많이 사용한다. 스프링에서도 이 Restful API 개발을 지원하는데 이때 쓰는 것이 @RestController 어노테이션이다. 위에서 작성한 Person 객체를 JSON 형태로 응답해주는 간단한 RestController를 만들어보자.

일단 com.example.demo.main.controller 패키지 안에 MainRestController.java 파일을 추가한다.

![start-spring-16](\assets\images\start-spring\start-spring-16.JPG)



그 다음 MainRestController 안에 아래와 같이 작성해주자.

```java
package com.example.demo.main.controller;

import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;
import org.springframework.web.bind.annotation.RestController;

import com.example.demo.main.vo.Person;

// RestController 어노테이션을 통해 이제부터 이 java 파일은 RestController 역할을 수행한다.
@RestController
public class MainRestController {
	@RequestMapping(value="/api", method=RequestMethod.GET)	// URL의 path와 HTTP method를 작성한다.
	public Person getPerson() {
		return new Person();	// 객체형으로 응답하면 JSON 형태로 변환된다.
	}
}

```



그 다음 http://localhost:8080/api 로 접속하여 Person 객체를 제대로 반환해주는 지 확인한다.

![start-spring-17](\assets\images\start-spring\start-spring-17.JPG)



## 추가 설정: port 변경

기본으로 사용되는 8080포트가 마음에 안들거나, 다른 포트를 사용해야 될 경우가 빈번하다. 그럴때는 **application.properties**파일에 설정을 추가해주면 된다. 아래와 같이 설정을 추가해보자.

```properties
server.port=8081
```



그 다음 서버를 재시작한 뒤 http://localhost:8081로 접속하여 제대로 접속이 되는 지 확인해보자.

![start-spring-18](\assets\images\start-spring\start-spring-18.JPG)



## 추가 설정: 코드 수정 시 서버 자동 재시작 (reload, hot load)

서버 코드를 수정하게 되면 java 부분은 당연히 재시작을 해야 반영이 되지만, jsp나 정적인 js, image 등등을 수정했을 때도 서버를 재시작해줘야 하는 번거로움이 있다. 이 번거로움을 해결하기 위해 스프링에선 devtools 라는 모듈을 제공하는데, 서버의 코드가 수정되면 자동으로 재시작 및 리로드를 해주는 모듈이다. *(java 수정시엔 자동 재시작, jsp나 정적 파일 수정 시엔 재시작 없이 자동 reload)*

해당 모듈은 pom.xml에 아래 코드를 추가하면 사용할 수 있다.

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-devtools</artifactId>
    <optional>true</optional>
</dependency>
```



모듈 적용을 위해 수동으로 서버 재시작 후, 코드를 수정해보자. 자동으로 서버가 재시작되거나 jsp 수정 후 브라우저 새로고침 시에 수정내용이 반영되면 모듈이 반영된 것이다.