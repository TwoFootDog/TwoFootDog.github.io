---
layout: post
title: "[Docker]Docker컨테이너에 jenkins 설치하기"
description: 
image: '../images/강아지61.jpg'
category: 'Docker'
tags : 
- IT
- Docker
- Jenkins

twitter_text: 
introduction : Docker컨테이너에 Jenkins 설치 후 Maven을 활용하여 Spring boot jar파일을 빌드해보자
---

### [1. 개요]
Redmine은 프로젝트의 할 일을 관리하는 도구이다. 할일이란 개발해야 할 새 기능, 수정해야 하는 결함, 문제가 된 이슈 등을 모두 포함한다. 최근에는 이 모든 것을 이슈(Issue)라고 통칭하고, Redmine과 같은 이슈를 관리하는 도구를 이슈 추적 시스템, 즉 Issue Tracking System(이하 ITS)이라 부른다. Redmine외에 Track, Mantis등이 대표적인 오픈소스 도구이며 Atlassian의 Jira는 대표적인 상용도구이다.

Redmine을 설치하기 위해서는 아래와 같이 작업을 진행하면 된다.



_ _ _

### [1. 사전작업]
- docker 설치
- git 설치


_ _ _

### [2. Docker컨테이너에 Jenkins 설치]
1. 도커 컨테이너에 Jenkins 설치 후 구동 : `docker run --rm -d -u root -p 8081:8080 -v jenkins-data:/var/jenkins_home -v /var/run/docker.sock:/var/run/docker.sock -v "$HOME":/home jenkins/jenkins:lts`
2. Jenkins 포트 방화벽 오픈 : 
	- `sudo iptables -I INPUT 1 -p tcp --dport 8081 -j ACCEPT `
	- `sudo iptables -I OUTPUT 1 -p tcp --dport 8081 -j ACCEPT `
3. 도커 컨테이너 이름 변경 : ` docker rename  heuristic_pascal jenkins-tutorials`
4. 도커 컨테이너 접속 : ` docker exec -it jenkins-tutorials bash`
5. jenkins 웹(<http://(젠킨스 서버 ip주소):(젠킨스 port)>) 접속 후 admin password 명령어 확인
6. admin password 명령어를 접속한 도커 컨테이너에 입력하여 admin password 확인 후 도커 웹에 입력 : `cat /var/jenkins_home/secrets/initialAdminPassword`
7. 

_ _ _


*출처 : 
- <https://jenkins.io/doc/tutorials/build-a-java-app-with-maven/#setup-wizard> 참고