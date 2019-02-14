---
layout: post
title: "[Spring]메이블(Maven) 개념 정리"
description: 메이븐(Maven) 개념 정리
image: '../images/강아지23.jpg'
category: 'Maven'
tags : 
- IT
- Spring
- SpringMVC
- Maven

twitter_text: 
introduction : 메이븐(Maven) 개념에 대해서 간단히 정리해보자.
---

1) 메이븐(Maven)이란?

라이브러리에 대한 의존 관계 관리 및 소스코드부터 배포 가능한 산출물(artifact)을 빌드(build)하는 **"빌드 툴(build tool)"**


_ _ _


2) Ant와의 공통점 및 차이점

공통점 : 코드 빌드와 배포를 주로 사용하는 소스 관리 툴
차이점 : 
- Ant : 절차지향적. 비교적 자유도가 높음(전처리/컴파일/패키징/테스팅/배포기능). 라이프사이클이 없음(글과 글에 대한 의존성 정의)
- Maven : 서술적. 정해진 라이프사이클에 의하여 작업 수행. 전반적인 프로제트 관리 기능까지 포함하고 있음.

_ _ _


3) 메이븐(Maven) 사용 시 이점
- 편리한 의존관계 라이브러리 관리
- 모든 프로젝트가 일관된 디렉토리 구조와 빌드 프로세스를 유지(메이븐은 이미 정형화된 프로젝트 구조와 빌드 명령 제공)
- 메이븐이 제공하는 다양한 플러그인 활용(플러그인을 이용한 다양한 통합개발 환경, 프로젝트 자동변환, 데이터베이스 통합 등 가능)
- 전시적으로 사용할 프로젝트 템플릿을 만들어 배포



_ _ _


4) 메이븐(Maven) 설치
- <http://maven.apache.org/download.cgi> 에서 메이븐 최신버전 다운로드
- 다운받은 압축파일을 원하는 경로에 풀고, 해당 경로를 시스템 환경변수에 **"MAVEN_HOME"** 추가
- 시스템 환경변수 **"PATH"**에 **"%MAVEN_HOME%/bin"** 추가(mvn 명령어를 편하게 사용하기 위함)
- 메이븐 기본 로컬 저장소 위치는 **"USER_HOME/.m2**이며, 메이븐 중앙 저장소는 **"https://repo.maven.apache.org/maven2/"** 이다.
- 중앙 저장소에서 로컬 저장소로 라이브러리나 플러그인을 다운로드 하는데, 로컬 저장소는 settings.xml의 <localRepository> 태그 주석 해제 후 변경 가능.



_ _ _



5) 메이븐(Maven) 설정파일(settings.xml)
- MAVEN_HOME/conf/settings.xml :  메이븐을 설치한 컴퓨터의 모든 사용자에 동일한 설정을 하기 위한 용도. 대부분 주석 처리되어 있으며 추가적인 설정이 필요하다면 주석을 참고하여 설정
- USER_HOME/.m2/settings.xml : 사용자별로 다른 설정이 필요하다면 해당 디렉토리에 settings.xml을 만들어 설정 가능.



_ _ _


6) 메이븐(Maven) POM 설정파일(pom.xml)
- 메이븐 기반 프로젝트에서 사용하는 프로젝트 내 빌드 설정파일.
- 기본구성요소 : 
   - 프로젝트 기본정보(프로젝트 이름, URL, 개발자, 라이선스 등)
   - 의존성 라이브러리 정보 : groupId, artifactId, version 정보 필요. A라는 라이브러리를 사용하는데 B,C,D가 의존성을 가진다면 A만 dependency에 추가하면 자동으로 B,C,D를 가져온다. dependency에는 <scope> 태그가 있는데 해당 라이브러리가 언제 필요한지 정의할 수 있다.(compile, runtime, provided, test 등)
   - 빌드 설정 및 환경 : 메이븐의 핵심인 빌드(build)와 관련된 정보를 설정함. (아래에서 자세히 설명)



_ _ _


7) 메이븐을 이용한 의존성 관리
- 




_ _ _





7) 메이븐(Maven) 빌드(build) & 라이프사이클 관련 설명
- 빌드란? 자바코드를 실제로 사용할 수 있게 정리하는 과정. compile, test, package, install, deploy 등이 이에 포함된다.
- 라이프사이클이란? 메이븐이 미리 정의하고 있는 빌드 순서. 라이프사이클의 각 빌드 단계를 페이즈(phase)라고 한다. 
- 페이즈(phase) 설명:
  - 기본 라이프사이클
  	- complie : 소스 파일을 컴파일한다.
  	- test : 단위테스트 실행(기본설정은 단우이테스트가 실패하며 빌드 실패로 간주함)
  	- package : 컴파일된 클래스 파일과 리소스 파일들을 war 혹은 jar와 같은 파일로 패키징
  	- install : 패키징한 파일을 로컬 저장소에 배포(USER_HOME/.m2)
  	- deploy : 패키징한 파일을 원격 저장소에 배포(nexus혹은 mavenn central 저장소)
  - clean 라이프사이클
    - clean : 메이븐 빌드를 통해 생성된 모든 산출물을 삭제(target 디렉토리에 생성된 산출물 삭제(예 : war파일 등))
  - site 라이프사이클
    - site : 메이븐 설정파일 정보를 활용하여 프로젝트에 대한 문서 사이트를 생성
    - site-deploy : 생성한 문서 사이트를 설정되어 있느 서버에 배포



_ _ _


8) 메이븐 플러그인(plugin)과 골(goal)
- maven의 모든 기능은 플러그인(plugin) 기반으로 동작한다. 메이븐 라이프사이클에 포함되어있는 페이즈 또한 플러그인을 통하여 실질적인 작업이 실행된다.
- <build><plugins><plugin> 태그를 사용하여 사용하고자 하는 플러그인을 추가 및 설정할 수 있다.
- 플러그인에서 실행할 수 있는 각각의 작업을 골(goal)이라고 하며 각 골은 maven 라이프사이클의 페이즈(phase)와 연결된다. 
- 플러그인 골(goal) 실행방법 : mvn groupId:artifactId:version:goal(너무 어렵네)
- 페이즈(phase) 별 플러그인 골(goal) 실행방법
  - 기본 라이프사이클
  	- mvn process-resources : resource 디렉토리에 있는 내용을 target/classes로 복사
  	- **mvn compile** : src/java 밑의 모든 자바소스를 컴파일해서 target/classes로 복사
  	- mvn process-testResources, mvn test-compile : 위 두 명령어가 src/java라면, 이것은 test/java 내용을 target/test-classes로 복사
  	- mvn test : target/test-classes에 있는 테스트케이스의 단위테스트를 진행한다. 결과는 target/surefie-reports에 생성
  	- **mvn package** : target디렉토리 하위에 jar, war, ear등 패키지파일을 생성한다. 이름은 pom.xml <build>태그의 <finalName>값을 사용한다.
  	- **mvn install** : 로컬 저장소(USER_HOME/.m2)로 packaging한 파일을 배포한다.
  	- **mvn deploy** : 원격 저장소로 packaging한 파일을 배포한다.
  - clean 라이프사이클
  	- **mvn clean** : 빌드 과정에서 생긴 target 디렉토리 내용을 삭제한다. mvn install 시 target을 지웠다가 다시 생성하고 싶으면 "mvn clean install" 명령어를 수행한다.
  - site 라이프사이클
  	- mvn site : target/site에 문서 사이트를 생성한다.
  	- mvn site-deploy : 문서 사이트를 서버로 배포한다.
- **페이즈(phase)는 특정 순서에 따라 goal이 실행되도록 구조를 제공하며, phase간에는 의존관계가 있다.(예를들면 mvn package가 수행되기 전에 mvn compile 등 이전 단계 goal이 실행된다.)**



_ _ _


9) pom.xml  <build> 태그 설명
- <finalMame> : 빌드 결과물(war, jar, ear) 이름 설정
- <resources> : 리소스(각종 설정 파일)의 위치 지정 가능. <resource>가 없으면 기본으로 "src/main/resources"
- <testResources> : 테스트 리소스의 위치 지정 가능. <testResource>가 없으면 기본으로 "src/test/resources"
- <repositories> : 빌드할 때 접근할 저장소의 위치를 지정할 수 있다. 기본적으로 메이븐 중앙 저장소인 <http://repo1.maven.org/maven2>로 지정되어 있음. 
- <outputDirectory> : 컴파일한 결과물 위치 값 지정. 기본 "target/classes"
- <testOutputDirectory> : 테스트 소스를 컴파일한 결과물 위치 값 지정. 기본 "target/test-classes"
- <plugin> : 어떤 액션 하나를 담당하는 것으로 가장 중요하며 다양한 옵션값이 들어갈 수 있음.
	- <executions> : 플러그인 goal과 관련된 실행에 대한 설정
	- <configuration> : 플러그인에서 필요한 설정 값 지정


*출처 : 
<https://dimdim.tistory.com/entry/Maven-%EC%A0%95%EB%A6%AC>

<https://jeong-pro.tistory.com/168>

참고
