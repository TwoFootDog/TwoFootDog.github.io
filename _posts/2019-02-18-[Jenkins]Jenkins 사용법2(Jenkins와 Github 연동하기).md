---
layout: post
title: "[Jenkins]Jenkins 사용법(Jenkins 환경셋팅 및 Github와 연동하기)"
description: Jenkins 환경셋팅 및 Github 연동하기
image: '../images/강아지25.jpg'
category: 'Jenkins'
tags : 
- IT
- Jenkins
twitter_text: 
introduction : Jenkins 환경설정 및 Github에 코드를 푸쉬하면 Jenkins가 이를 인지해서 자동으로 코드를 내려 받아서 빌드 스크립트가 실행되게끔 설정해보자.
---

젠킨스 설치가 완료되었으니 이번 포스트에서는 환경셋팅 및 Github와의 연동을 진행해본다.



_ _ _



##Manage Users

1) **http://(서버ip):(젠킨스port)** 로 접속한다.
2) 좌측 메뉴의 **Jenkins관리 -> Manage Users -> 사용자 생성**으로 들어가서 사용자를 새로 생성해준다.
![첫번째이미지](../images/jenkins2_20190218_1.jpg)


_ _ _



##Configure Global Security

1) **Jenkins관리 -> Configure Global Security** 선택

2) 사용자 가입을 허용하기 위해 **Security Realm**의 **Jenkins own user database**를 선택하고, 사용자의 가입허용 체크박스에 체크한다.

3) 사용자 별 권한을 관리하기 위해 Authorization의 Matrix-based-security를 선택한다.

4) Add user or group으로 존재하는 계정을 입력한다.

5) 권한 설정 제일 오른쪽에 있는 체크박스는 전체선택, 전체해제이다.

![두번째이미지](../images/jenkins2_20190218_2.jpg)
![세번째이미지](../images/jenkins2_20190218_3.jpg)




_ _ _



##Global Tool Configuration

1) 젠킨스에서 사용하는 JDK, Git, Maven 설치 및 설정 정보를 입력한다.
- JDK 설치
	-`apt-cache search jdk` : 우분투 리눅스 서버에 JDK 검색
	- `apt-get install openjdk-8-jdk` : JDK8 버전 설치
	- `/usr/lib/jvm/java-8-openjdk-amd64` : JDK 설치 디렉토리
- Git 설치
	- `sudo apt-get install git-core` : 우분투 리눅스 서버에 git 설치
	- `/usr/bin/git` : git 설치 디렉토리
- Maven 설치
	- `sudo apt install maven` : 우분투 리눅스 서버에 maven 설치
	- `mvn -v` : maven 설치 확인 및 정보 확인
	- `/usr/share/maven` : maven 설치 디렉토리

2) 설정정보 입력(Jenkins관리 -> Global Tool Configuration -> JDK, Git, Maven 설치 디렉토리 입력)
![네번째이미지](../images/jenkins2_20190218_4.jpg)
![다섯번째이미지](../images/jenkins2_20190218_5.jpg)

_ _ _



##시스템 설정
1) 우선 <https://github.com>에서 Github API Token을 생성한다.(**Profile 클릭 -> Settings -> Developer settings -> Personal access tokens -> Generate new token**)
![여섯번째이미지](../images/jenkins2_20190218_6.jpg)
![일곱번째이미지](../images/jenkins2_20190218_7.jpg)

2) Token description을 입력하고, select scops에 repo, admin:repo_hook을 선택한 후 Generate token을 클릭한다.
![여덟번째이미지](../images/jenkins2_20190218_8.jpg)

3) 아래와 같이 Token이 생성된다. Token 확인 후 복사한다.
![아홉번째이미지](../images/jenkins2_20190218_9.jpg)

4) jenkins로 돌아와서 **jenkins 관리 -> 시스템 설정 -> Github** 에서 Github Servers를 추가하고 정보를 입력한다.
![열번째이미지](../images/jenkins2_20190218_10.jpg)
![열한번째이미지](../images/jenkins2_20190218_11.jpg)


_ _ _

*출처 : 
- <https://dukeom.wordpress.com/2017/03/20/jenkinsgithubmaven-%EC%9C%BC%EB%A1%9C-%EB%B9%8C%EB%93%9C%EB%B0%B0%ED%8F%AC%ED%95%98%EA%B8%B0-24/>
