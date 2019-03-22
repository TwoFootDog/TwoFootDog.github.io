---
layout: post
title: "[Docker]Docker-compose와 Dockerfile을 활용한 nginx 서버 구축"
description: 
image: '../images/강아지48.jpg'
category: 'Docker'
tags : 
- IT
- Docker
- nginx
- Web

twitter_text: 
introduction : Docker-compose와 Dockerfile을 활용하여 간단한 nginx 서버를 구축해보자
---

### [1. 개요]
Docker를 활용하면 ngix를 간단하게 설치할 수 있다. 여기서 더 나아가 Dockerfile을 활용하여 개별적으로 이미지를 만들어서 nginx를 구동할 수 있다. 그렇다면 Docker-compose란 무엇인가? Docker-compose의 정의에 대해서 알아보고 Docker-compose와 Dockerfile을 활용하여 nginx 서버를 구축해보도록 하자.


**1. Docker-compose란**
복수개의 컨테이너가 하나의 애플리케이션으로 구동되는 경우 이 애플리케이션을 테스트하기 위해서는 역할별로 각각 하나의 컨테이너를 생성해야 한다. 예를 들어 규모가 있는 웹 애플리케이션일 경우 Web 서버 컨테이너, WAS 서버 컨테이너, DB 서버 컨테이너등을 생성해야 한다. 이 애플리케이션 구동을 위해서 'docker run' 명령어를 여러번 사용하여 컨테이너를 구동하고 개별적인 컨테이너가 정상적으로 동작하는지 확인하는 단계가 필요하다. 그러나 매번 이러한 단계등을 위해서 run 명령어에 옵션을 설정하여 CLI로 컨테이너를 생성하는 것은 비효율적이다. 하나의 서비스를 위해서 여러개의 컨테이너를 개별 서비스로 정의하여 컨테이너의 묶음으로 관리하게 된다면 무척 편리할 것이고, 이러한 개념으로 실제 구현된 것이 **도커 컴포즈(Docker Compose)** 인 것이다. 도커 컴포즈(Docker Compose)는 컨테이너를 이용한 서비스의 개발과 CI(Continous Integration)를 위하여 여러개의 컨테이너를 하나의 프로젝트로 다룰 수 있는 작업환경을 제공한다. 

도커 컴포즈(Docker Compose)는 여러개의 컨테이너 옵션과 환경을 정의한 파일을 읽어 컨테이너를 순차적으로 생성하는 방식으로 동작한다. 도커 컴포즈의 설정 파일은 run 명령어의 옵션을 그대로 사용할 수 있으며, 각 컨테이너의 의존성, 네트워크, 볼륨 등을 함께 정의할 수 있다. 뿐만 아니라 스웜모드의 서비스와 유사하게 설정파일에 정의된 서비스의 컨테이너 수를 유동적으로 조절할 수 있으며, 컨테이너의 서비스 Discovery(클라이언트나 api게이트웨이가 호출할 서비스를 찾는 메커니즘)도 자동으로 이루어진다. 소규모 형태의 서비스나 컨테이너 개발 환경에서는 도커 엔진의 run 명령어로 컨테이너를 생성하는 것이 편리할 수 있지만 컨테이너의 수가 많아지고 정의해야 할 옵션이 많아진다면 도커 컴포즈(Docker Compose)를 사용하는 것이 좋다.


**2. Dockerfile이란**
[클릭하기](https://twofootdog.github.io/Docker-Dockerfile%EC%A0%95%EC%9D%98-%EB%B0%8F-%EC%9E%91%EC%84%B1%EB%B2%95-%EA%B0%80%EC%9D%B4%EB%93%9C/)




_ _ _




### [2. Dockerfile와 docker-compose를 활용한 nginx 서버 구축]


**1. nginx.conf 및 index.html 작성**
- ~/docker/ 디렉토리에서 **nginx.conf** 파일 작성
- ![](../images/dockercompose_20190321_3.jpg)
- ~/docker/디렉토리에서 **index.html** 파일 작성
- ![](../images/dockercompose_20190321_6.jpg)




**2. Dockerfile 작성**
- ~/docker/ 디렉토리에 **Dockerfile.nginx** 파일 작성
- ![](../images/dockercompose_20190321.jpg)




**3. Docker-Compose 설치**
수동으로 설치를 하려면 아래 명령어를 실행하면 된다.(<https://docs.docker.com/compose/install/#prerequisites> 참고)
- `sudo curl -L "https://github.com/docker/compose/releases/download/1.23.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose`
- `sudo chmod +x /usr/local/bin/docker-compose`




**4. docker-compose.yml 파일 작성**
Dockerfile.nginx와 동일한 디렉토리야 **docker-compose.yml**파일을 작성
- version : 도커 버전에 맞는 버전을 작성한다. 버전은 (<https://docs.docker.com/compose/compose-file/>)에서 확인 가능하다
- build : 해당 항목에서 정의된 도커파일에서 이미지를 빌드하여 서비스의 컨테이너를 생성하도록 설정한다
- Dockerfile : 도커파일명
- ports : "docker run" 명령어의 -p 옵션과 같으며 서비스의 컨테이너를 개발할 포트 지정
![](../images/dockercompose_20190321_2.jpg)




**5. docker-compose 실행**
- 새로운 변경사항 반영 : `docker-compose build`
- 서비스 실행(컨테이너 생성) : `docker-compose up`
- **변경사항 반영 & 서비스 실행 & 콘솔화면에서 detach** : `docker-compose up -d --build`
- 실행
![](../images/dockercompose_20190321_4.jpg)




**5. 결과 확인**
![](../images/dockercompose_20190321_5.jpg)
![](../images/dockercompose_20190321_7.jpg)



_ _ _

*출처 : 
- <http://avilos.codes/infra-management/virtualization-platform/docker/docker-compose/> 
- <https://gompro.postype.com/post/1735800> 참고