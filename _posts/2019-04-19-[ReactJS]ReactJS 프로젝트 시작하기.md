---
layout: post
title: "[ReactJS]ReactJS 프로젝트 시작하기"
description: 
image: '../images/강아지62.jpg'
category: 'ReactJS'
tags : 
- IT
- ReactJS

twitter_text: 
introduction : ReactJS 프로젝트 시작하기
---

### [1. 개요]
ReactJS 프로젝트를 시작해보자.

_ _ _

### [2. 사전작업]
1. npm 설치
- node.js만 설치하면 npm이 자동으로 설치된다. node.js 사이트에서 node.js installer를 다운로드 한다.(<http://www.nodejs.org>)
- 설치 후 `npm -v` 명령어로 정상 설치 여부 및 버전을 확인한다.
2. VSCode 설치
- <https://code.visualstudio.com/> 에서 설치를 한다



_ _ _

### [3. create-react-app 설치]
1. 프로젝트 폴더를 생성하고자 하는 폴더로 가서 아래와 같이 명령어를 입력한다(프로젝트 명 : react_test). npx는 npm 패키지를 로컬에 글로벌로 설치하지 않고 바로 일회성으로 실행할 수 있게 해주는 도구이며 npm 5.2.0버전 이후부터 기본으로 제공된다. 
- 명령어 : `npx create-react-app react-test`

npx가 실행되지 않는 구버전인 경우는 아래와 같이 실행한다


- 명령어 : 
	- `npm install -g create-react-app`
	- `create-react-app react-test`

2. 설치가 완료되면 하위에 react-test 폴더가 생성되며 해당 폴더로 이동 후 프로젝트를 로컬에서 실행해본다.
- 명령어 : `npm start`

_ _ _

### [4. yarn을 통해 설치하는 방법]
1. yarn 설치
- 명령어 : npm install -g yarn

2. yarn을 통해서 create-ract-app 설치
- 명령어 : yarn global add create-react-app

3. create-react-app을 통해 프로젝트 생성
- 명령어 : create-react-app [프로젝트명(폴더명)]

4. yarn을 통해 프로젝트 구동
- 명령어 : yarn start




_ _ _





*출처 : 
- <https://eunvanz.github.io/react/2018/06/05/React-create-react-app%EC%9C%BC%EB%A1%9C-%ED%94%84%EB%A1%9C%EC%A0%9D%ED%8A%B8-%EC%8B%9C%EC%9E%91%ED%95%98%EA%B8%B0/> 
- <https://web-front-end.tistory.com/3>
참고