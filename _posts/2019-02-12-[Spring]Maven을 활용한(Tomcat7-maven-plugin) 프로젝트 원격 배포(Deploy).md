---
layout: post
title: "[Spring]Tomcat7-maven-plugin을 활용하여 Maven 기반 프로젝트(WAR파일)를 Tomcat에 배포하기(Deploy)"
description: 윈도우 환경에서 Tomcat7-maven-plugin을 활용하여 Maven 기반 프로젝트를 WAR로 Tomcat에 배포하기(Delploy)
image: '../images/강아지22.jpg'
category: 'Maven'
tags : 
- IT
- Spring
- SpringMVC
- Maven
- Tomcat
- Deploy
twitter_text: 
introduction : 윈도우 환경에서 Tomcat7-maven-plugin을 활용하여 Maven 기반 프로젝트를 WAR로 Tomcat에 배포해보자.
---

톰캣(Tomcat)은 서블릿 컨테이너 구현체 중 하나다. 개발자가 Java로 제작한 서블릿을 WAR로 압축하여 Tomcat의 웹 어플리케이션 루트인 **톰켓디렉토리/webapps** 디렉토리(필자는 C:\util\apache-tomcat-9.0.13\webapps)에 올리면 자동으로 압축을 풀어 서블릿으로 등록하고 관리해준다.(참고로 자동 압축 풀기는 톰켓 옵션으로 설정할 수 있습니다.)
FTP로 tomcat/webapps에 올리기만 하면 톰켓이 자동으로 압축을 풀어 앱으로 등록하기 때문에, FTP도 WAR 배포 수단이 될 수 있다.
다른 방법으로는 **톰켓 어플리케이션 관리자 도구**를 활용하여 배포할 수 있다. 지금부터 해당 방법에 대해서 소개하겠다.


1) tomcat을 구동해보자. tomcat 설치 후 path가 잡혀있으면(CATALINA_HOME 등) cmd를 실행시키고 **startup** 명령어를 실행하면 tomcat이 구동된다.


_ _ _


2) 인터넷 브라우저에 **localhost:8080/manager**로 접속해보자. 사용자이름/비밀번호 입력창이 나오는데 아직 설정을 해 주지 않았기 때문에 입력을 할 수가 없다. 가볍게 무시해주자.
![첫번째 이미지](../images/maven_deploy_20190212_1.jpg)


_ _ _


3) 톰켓디렉토리/conf에 있는 tomcat-user.xml 파일의 <tomcat-users> 태그 안에 아래 내용을 추가해주자.
manger-gui는 tomcat 관리자 기능을 웹페이지에서 볼수 있는 role이고, manager-script는 war파일을 관리자를 통해 배포가 가능하도록 한다. 사용자로는 admin을 등록했고, 해당 사용자는 manager-gui, manager-script등을 사용할 수 있다.
```
    <role rolename="manager-script"/>
    <role rolename="manager-gui"/>
    <role rolename="manager-jmx"/>
    <role rolename="manager-status"/>
    <user username="admin" password="admin" roles="manager-gui,manager-script,manager-status,manager-jmx"/>
```
![두번째 이미지](../images/maven_deploy_20190212_2.jpg)


단 위 방식으로 localhost나 127.0.0.1에서만 접근 가능하다고 한다. 때문에 원격지에서 접근을 하려면 한가지 설정을 더 해야 한다.
톰켓디렉토리/conf/Catalina/localhost/에 manager.xml을 만들고 아래 내용을 입력한다.(보안상으론 위험하지만 테스트니깐~)
```
<Context privileged="true" antiResourceLocking="false" docBase="${catalina.home}/webapps/manager">
        <Valve className="org.apache.catalina.valves.RemoteAddrValve" allow="^.*$"/>
</Context>
```


_ _ _


4) tomcat 중지 후 재기동한다(tomcat 중지 명령어 : 톰캣경로/bin/shutdown, tomcat 기동 명령어 : startup).
재기동 후 **localhost:8080/manager**로 사용자이름/비밀번호 입력후 재 접속해보자. 아래와 같이 접속이 되는 것을 확인할 수 있다.
![세번째 이미지](../images/maven_deploy_20190212_3.jpg)




```
  <build>
        <finalName>nxmile_approval</finalName>
        <plugins>
            <!-- 배포 -->
            <plugin>
                <groupId>org.apache.tomcat.maven</groupId>
                <artifactId>tomcat7-maven-plugin</artifactId>
                <version>2.2</version>
                <configuration>
                    <url>http://localhost:8080/manager/text</url>
                    <server>TomcatServer</server>
                    <path>/${project.build.finalName}</path>
                    <username>admin</username>
                    <password>admin</password>
                </configuration>
            </plugin>
```



mvn tomcat7:redeploy


*출처 : 
<https://www.bsidesoft.com/?p=7123>
<https://wonzopein.com/56>
참고
