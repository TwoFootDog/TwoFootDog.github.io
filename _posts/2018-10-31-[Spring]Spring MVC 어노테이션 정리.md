---
layout: post
title: "[Spring]Spring MVC 어노테이션 정리"
description: Spring MVC 어노테이션 정리
image: '../images/강아지30.jpg'
category: 'SpringMVC'
tags : 
- IT
- Spring
- SpringMVC

twitter_text: 
introduction : Spring MVC 어노테이션 정의 및 자주 사용하는 어노테이션에 대해 알아보자.
---


### [어노테이션(Annotation)이란?]
Spring MVC에서는 어노테이션(Annotation)을 활용하여 개발을 수행한다. 어노테이션이란 주석이라는 사전적 의미를 가진 단어로, 스프링에서는 클래스, 필드, 메서드 같은 프로그램 요소에 다양한 종류의 정보를 주는 방법을 일컫는다. 한마디로 데이터를 위한 데이터, 즉 메타데이터이다. 어노테이션은 컴파일 혹은 런타임 시 해석이 된다. 
- 장점 : 코드량 감소, 생산성 증가
- 단점 : 가독성이 떨어짐

_ _ _



### [어노테이션 종류]

##### @Component
- 패키지 : org.springframework.stereotype
- 버전 : sprign 2.5
- 설정위치 : 클래스 선언부 앞
- 설명 : <context:component-scan> 태그를 xml 설정파일에 추가하고 적용하려는 패키지를 base-package에 명시하면, base-package를 검색하여 해당 어노테이션이 적용된 클래스를 빈으로 등록하게 된다. 범위는 디폴트로 singletone이며 @Scope를 사용하여 지정할 수 있다. 





_ _ _


##### @Required
- 패키지 : org;.springframework.beans.factory.annotation
- 버전 : spring 2.0
- 설정위치 : setter 메서드 앞
- 설명 : Required 어노테이션은 필수 프로퍼티임을 명시하는 것으로 필수 프로퍼티를 설정하지 않을 경우 빈 생성 시 예외를 발생시킨다.







_ _ _



*출처 : 
- 