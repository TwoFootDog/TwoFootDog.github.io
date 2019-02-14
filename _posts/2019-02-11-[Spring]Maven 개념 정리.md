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



7) 메이븐(Maven) 빌드(build) & 라이프사이클 관련 설명
- 빌드란? 자바코드를 실제로 사용할 수 있게 정리하는 과정. compile, test, package, install, deploy 등이 이에 포함된다.
- 라이프사이클이란? 메이븐이 미리 정의하고 있는 빌드 순서. 라이프사이클의 각 빌드 단계를 페이즈(phase)라고 한다. maven의 모든 기능은 플러그인(plugin) 기반으로 동작. 플러그인에서 실행할 수 있는 각각의 작업을 골(goal)이라고 하며 각 골은 maven 라이프사이클의 페이즈(phase)와 연결된다. 
- 페이즈 설명:
  - complie : 소스 파일을 컴파일한다.
  - test : 단위테스트 실행(기본설정은 단우이테스트가 실패하며 빌드 실패로 간주함)
  - 



*출처 : 
<https://www.bsidesoft.com/?p=7123>

<https://all-record.tistory.com/185>

참고
