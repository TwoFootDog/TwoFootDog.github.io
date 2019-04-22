---
layout: post
title: "[Docker]Docker컨테이너로 jenkins 실행 후 gitlab으로 React 소스 push 시 Docker Container로 자동 빌드/배포"
description: 
image: '../images/강아지63.jpg'
category: 'Docker'
tags : 
- IT
- Docker
- Jenkins
- React

twitter_text: 
introduction : Docker컨테이너로 jenkins 실행 후 gitlab으로 spring boot 소스 push 시 Docker Container로 자동 빌드/배포
---

### [1. 개요]
Docker 컨테이너로 jenkins 실행 후 gitlab에서 관리하는 React 소스를 수정 후 push 시 npm install 후 npm run build 하여 ./build 디렉토리에 결과물을 만들어서 Docker Container로 배포까지 수행해 보자.



_ _ _


*출처 : 
- <https://oofbird.net/35> 
참고