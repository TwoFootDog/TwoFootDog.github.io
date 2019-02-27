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

#### @Component
- 패키지 : org.springframework.stereotype
- 버전 : sprign 2.5
- 설정위치 : 클래스 선언부 앞
- 설명 : <context:component-scan> 태그를 xml 설정파일에 추가하고 적용하려는 패키지를 base-package에 명시하면, base-package를 검색하여 해당 어노테이션이 적용된 클래스를 빈으로 등록하게 된다. 범위는 디폴트로 singletone이며 @Scope를 사용하여 지정할 수 있다. 





_ _ _


#### @Required
- 패키지 : org;.springframework.beans.factory.annotation
- 버전 : spring 2.0
- 설정위치 : setter 메서드 앞
- 설명 : Required 어노테이션은 필수 프로퍼티임을 명시하는 것으로 필수 프로퍼티를 설정하지 않을 경우 빈 생성 시 예외를 발생시킨다. RequiredAnnotationBeanPostProcessor 클래스는 스프링 컨테이너에 등록된 bean 객체를 조사하여 @Required 어노테이션으로 설정되어 있는 프로퍼티의 값이 설정되어 있는지 검사한다.
사용하려면 <bean class="org.springframework.beans.factory.annotation.RequiredAnnotationBeanPostProcessor" /> 클래스를 빈으로 등록시켜줘야 하지만 이를 대신하여 <context:annotation-config> 태그를 사용해도 된다:





_ _ _


#### @Autowired
- 패키지 : org.springframework.beans.factory.annotation
- 버전 : spring 2.5
- 설정 위치 : 생성자, 필드, 메서드(setter 메서드가 아니어도 됨) 앞
- 설명 : 의존관계를 자동설정할 때 사용하며, 타입을 이용하여 의존하는 객체를 삽입해 준다. 그러므로 해당 타입의 빈 객체가 존재하지 않거나 또는 2개 이상 존재할 경우 예외를 발생시킨다. 사용하려면 <bean class="org.springframework.beans.factory.annotation.AutowiredAnnotationBeanPostProcessor" /> 클래스를 빈으로 등록시켜줘야 한다. 해당 설정 대신에 <context:annotation-config> 태그를 사용해도 된다.
@Autowired를 적용할 때 같은 타입의 빈이 2개 이상 존재하게 되면 예외가 발생하는데, Autowired도 이러한 문제가 발생한다. 이럴 때 @Qualifier를 사용하면 동일한 타입의 빈 중 특정 빈을 사용하도록 하여 문제를 해결할 수 있다.



_ _ _


#### @Qualifer
- 패키지 : org.springframework.beans.factory.annotation
- 버전 : spring 2.5
- 설정 위치 : @Autowired 어노테이션과 함께 사용
- 설명 : qualifier 어노테이션은 @Autowired의 목적에서 동일 타입의 빈객체가 존재시 특정빈을 삽입할 수 있게 설정한다. @Qualifier("mainBean")의 형태로 @Autowired와 같이 사용하며 해당 <bean>태그에 <qualifire value="mainBean" /> 태그를 선언해주어야 한다. 메서드에서 두개이상의 파라미터를 사용할 경우는 파라미터 앞에 선언해야한다.



_ _ _


#### @Resource
- 패키지 : javax.annotation.Resource
- 버전 : spring 2.5
- 설명 : @Autowired와 흡사하지만 @Autowired는 타입으로(by type), @Resource는 이름으로(by name)으로 연결한다는 점이 다르다. 사용하려면 <bean class="org.springframework.beans.factory.annotation.CommonAnnotationBeanPostProcessor"/> 클래스를 빈으로 등록시켜줘야 한다. 해당 설정 대신에 <context:annotation-config> 태그를 사용해도 된다.





_ _ _





*출처 : 
- <https://s262701-id.tistory.com/91> 참고