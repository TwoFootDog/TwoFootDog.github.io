---
layout: post
title: "[Spring]Spring MVC에서 properties 파일을 이용하여 설정하기"
description: Spring MVC 프로젝트에서 properties 파일을 이용하여 설정하기
image: '../images/강아지16.jpg'
category: 'SpringMVC'
tags : 
- IT
- Spring
- SpringMVC
- Properties

twitter_text: 
introduction : Spring MVC 프로젝트에서 properties 파일을 이용하여 설정하기
---

Spring 프로젝트를 진행하다 보면 Properties 파일에 변하지 않는 상수값나 텍스트(예를 들면 DB 접속정보 등)를 저장해 놓고 xml이나 java파일에서 사용하는 경우가 많이 있다.

Spring MVC 프로젝트에 Properties 파일을 적용해보자.

_ _ _


1) applicationContext.xml(혹은 root-context.xml) 파일에 property-placeholder를 작성한다.
```
<context:property-placeholder location="classpath:config/database.properties" />

```








_ _ _


2) src/main/resources/config에 database.properties 파일을 작성한다.(기본적으로 classpath가 src/main/resources로 잡혀 있어야 한다.)

```
hikcariConfig.driverClassName=net.sf.log4jdbc.sql.jdbcapi.DriverSpy
hikariConfig.jdbcUrl=jdbc:log4jdbc:mariadb://127.0.0.1:3306/test
hikariConfig.username=root
hikariConfig.password=1111
```



_ _ _


3) applicationContext.xml에서 properties에 등록된 변수를 사용할 경우 ${변수명}으로 사용한다.

```
    <!-- HikariCP connection pool bean 셋팅-->
    <bean id="hikariConfig" class="com.zaxxer.hikari.HikariConfig">
        <!--<property name="driverClassName" value="org.mariadb.jdbc.Driver"></property>-->
        <!--<property name="jdbcUrl" value="jdbc:mariadb://127.0.0.1:3306/test"></property>-->
        <property name="driverClassName" value="${hikcariConfig.driverClassName}"></property>
        <property name="jdbcUrl" value="${hikariConfig.jdbcUrl}"></property>
        <property name="username" value="${hikariConfig.username}"></property>
        <property name="password" value="${hikariConfig.password}"></property>
    </bean>
```




_ _ _


4) java파일에서 properties에 등록된 변수를 사용할 경우 Value 어노테이션을 사용한다.

```
@Value(hikariConfig.username)
private String username;

@Value(hikariConfig.password)
private String password;
```


*출처 : <http://jijs.tistory.com/entry/spring-%EC%84%A4%EC%A0%95-xml%EA%B3%BC-%EC%86%8C%EC%8A%A4%EC%BD%94%EB%93%9C%EC%97%90%EC%84%9C-properties-%EC%82%AC%EC%9A%A9%ED%95%98%EA%B8%B0> 참고
