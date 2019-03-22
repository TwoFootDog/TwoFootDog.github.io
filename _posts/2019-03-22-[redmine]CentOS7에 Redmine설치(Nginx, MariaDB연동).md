---
layout: post
title: "[redmine]CentOS7에 Redmine 설치(Nginx, MariaDB 연동)"
description: 
image: '../images/강아지51.jpg'
category: 'Redmine'
tags : 
- IT
- Redmine
- CentOS

twitter_text: 
introduction : CentOS7에 Redmine을 설치해보고 Nginx, MariaDB와 연동하여 실행시켜보자
---

### [1. 개요]
Redmine은 프로젝트의 할 일을 관리하는 도구이다. 할일이란 개발해야 할 새 기능, 수정해야 하는 결함, 문제가 된 이슈 등을 모두 포함한다. 최근에는 이 모든 것을 이슈(Issue)라고 통칭하고, Redmine과 같은 이슈를 관리하는 도구를 이슈 추적 시스템, 즉 Issue Tracking System(이하 ITS)이라 부른다. Redmine외에 Track, Mantis등이 대표적인 오픈소스 도구이며 Atlassian의 Jira는 대표적인 상용도구이다.

Redmine을 설치하기 위해서는 아래와 같이 작업을 진행하면 된다.



_ _ _

### [1. 사전작업(필수 패키지 설치)]
- epel-release 설치
	- epel-release 설치 확인 : `yum list epel-release` 
	- 설치 안되어 있으면 : `yum install epel-release`
- nginx 설치 : [nginx설치](https://twofootdog.github.io/nginx-nginx%EC%9D%98-%EC%9D%B4%ED%95%B4-%EB%B0%8F-CentOS7%EC%97%90-nginx-%EC%84%A4%EC%B9%98%ED%95%98%EA%B8%B0/)
- mariadb 설치 : [mariadb설치](https://twofootdog.github.io/MariaDB-CentOS7%EC%97%90%EC%84%9C-MariaDB-%EC%84%A4%EC%B9%98-%EB%B0%8F-%ED%99%98%EA%B2%BD%EC%84%A4%EC%A0%95/)
- ruby 설치

_ _ _


*출처 : 
- <https://zetawiki.com/wiki/CentOS_%EB%A0%88%EB%93%9C%EB%A7%88%EC%9D%B8_%EC%84%A4%EC%B9%98> 
- <https://wlsufld.tistory.com/40>  참고