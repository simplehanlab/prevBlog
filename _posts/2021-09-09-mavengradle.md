---

title: "Maven과 Gradle 차이"
comments: true
categories:
  - build
tags:
  - maven
  - gradle
date: "2021-09-09 16:00"
---

# 빌드 도구란?

- 빌드 도구는 빌드 자동화를 시켜주는 실행가능한 프로그램만들어 주는 프로그램 ex) maven, gradle, ant

## 빌드 자동화란?
- 빌드 자동화의 본질적 목적은 소프트웨어 개발자가 반복해서 하는 코딩을 잘 짜여진 프로세스를 통해 자동으로 실행하여, 
믿을 수 있는 결과물도 생산해 낼 수 있는 일련의 작업방식 및 방법을 말한다.

- 지속적인 통합(continuous Integration)과도 일맥상통되는 의미다. 
수없이 반복해서 최종 결과물을 만들어내야 하는 소프트웨어 개발업무에서 빠질 수 없는 개념적 이해가 되었다. 
특히 오늘날의 복잡하고 광범위한 협업적 개발 과정에서는 과정과정에서 있을 수 있는 인간적 실수의 가능성을 최소한으로 줄여야 한다. 

출처 : Wikipedia

## Maven 이란?
- Project object model (POM)개념의 기반으로 만들어져있으며 빌드 중인 프로젝트, 빌드 순서, 
다양한 외부 라이브러리의 종속성 관계들을 pom.xml파일에 명시한다. 
- 외부저장소에서 필요한 라이브러리와 플러그인들을 다운로드 한 다음, 로컬시스템의 캐시에 모두 저장한다.

### Project Object Model (POM)
Project Object Model는 이름 그대로 Project Object Model의 정보를 담고있는 파일이다.   
이 파일에서 주요하게 다루는 기능들은 다음과 같다.
- 프로젝트 정보 : 프로젝트의 이름, 개발자 목록, 라이센스 등
- 빌드 설정 : 소스, 리소스, 라이프 사이클별 실행한 플러그인(goal)등 빌드와 관련된 설정
- 빌드 환경 : 사용자 환경 별로 달라질 수 있는 프로파일 정보
- POM연관 정보 : 의존 프로젝트(모듈), 상위 프로젝트, 포함하고 있는 하위 모듈 등


## Gradle 이란?
- Maven과 Ant에서 볼수 있는 개념들을 사용하는 대안으로써 나온 프로젝트 빌드 관리 툴이다.
- Ant처럼 유연한 범용 빌드 도구
- Maven을 사용할 수 있는 변환 가능 컨벤션 프레임 워크
- Groovy 언어를 사용한 Domain-specific-language를 사용한다. 
(설정파일을 xml파일을 사용하는 Maven보다 코드가 훨씬 간결하다.)
- 2007년에 처음 개발되었고, 2013년에 구글에 의해 안드로이드 프로젝트의 빌드 시스템으로 채택되었다.

## Maven Gradle 차이
1. Maven은 고정적이고 선형적인 단계의 모델을 기반  
Gradle은 작업 의존성 그래프를 기반
2. Maven, Gradle 둘 다 다중 모듈 빌드를 병렬로 실행할 수 있지만  
Gradle은 어떤 task가 업데이트 되었고 안되었는지를 체크하기 때문에 incremental build를 허용한다.  
이미 업데이트된 task에 대해서는 작업이 실행 되지 않으므로 빌드 시간이 훨씬 단축된다.
3. Maven은 멀티 프로젝트에서 특정 설정을 다른 모듈에서 사용하려면 상속을 받아야하지  
Gradle은 설정 주입 방식을 제공한다.
4. Gradle은 concurrent에 안전한 캐시를 허용한다.  
2개 이상의 프로젝트에서 동일한 캐시를 사용할 경우,     
서로 overwrite되지 않도록 checksum기반의 캐시를 사용하고 캐시를 repository와 동기화시킬 수 있다.

## Maven 코드 예제
```terminal
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
<modelVersion>4.0.0</modelVersion>

    <groupId>com.example</groupId>
    <artifactId>demo-maven</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <packaging>jar</packaging>

    <name>demo-maven</name>
    <description>Demo project for Spring Boot</description>

    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>1.5.4.RELEASE</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>

    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
        <java.version>1.8</java.version>
    </properties>
    
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter</artifactId>
        </dependency>
    
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>
</project>
```

## Gradle 코드 예제
```terminal
buildscript {
    ext {
        springBootVersion = '1.5.4.RELEASE'
    }
    
    repositories {
        m  avenCentral()
    }
    dependencies {
        classpath("org.springframework.boot:spring-boot-gradle-plugin:${springBootVersion}")
    }
}

apply plugin: 'java'
apply plugin: 'idea'
apply plugin: 'org.springframework.boot'

version = '0.0.1-SNAPSHOT'
sourceCompatibility = 1.8

repositories {
    mavenCentral()
}


dependencies {
    compile('org.springframework.boot:spring-boot-starter')
    testCompile('org.springframework.boot:spring-boot-starter-test')
}
```