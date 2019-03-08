---
layout: post
title: "[MongoDB]MongoDB 설치 및 환경설정(Windows10)"
description: 
image: '../images/강아지32.jpg'
category: 'MongoDB'
tags : 
- IT
- MongoDB
- NoSQL

twitter_text: 
introduction : MongoDB를 Windows10에서 설치하고 환경설정을 해보자.
---

### [MongoDB란?]
MongoDB는 도큐먼트(Document) 지향 데이터베이스 시스템이다. 흔히 NoSQL이라고 하는데 많은 NoSQL 중에서 가장 인기가 많은 시스템이다.

MongoDB를 아래와 같이 설치 및 환경설정을 해보도록 하자.
_ _ _




### [MongoDB 설치하기 및 환경설정]

1) [MongoDB 다운로드 사이트](https://www.mongodb.com/download-center/community) 접속. OS는 Windows, package는 MSI로 다운로드
![1](../images/mongodb_20190308_1.jpg)


_ _ _



2) msi파일을 실행하여 mongodb 설치. (setup type은 custom으로 선택하여 디렉토리 별도 지정함)
MongoDB 를 쉽게 관리하기 위한 GUI 툴인 compass는 설치 안함(추후 robomongo로 실습할 예정)
![2](../images/mongodb_20190308_2.jpg)
![3](../images/mongodb_20190308_3.jpg)
![4](../images/mongodb_20190308_4.jpg)


_ _ _



3) 환경변수 추가
- mongod라는 실행파일이 있는데 커맨드라인에서 경로를 들어가지 않아도 바로 실행할 수 있도록 환경변수를 추가한다.
![5](../images/mongodb_20190308_5.jpg)


_ _ _



4) mongodb 실행
- Windows키 + R -> cmd 실행하여 **mongod** 명령어 입력. **C:\data\db\** 폴더가 없기 때문에 아래와 같은 에러메시지가 발생한다.**
해당 경로에 폴더를 만들어줘도 되고, 직접 경로를 지정해줘도 된다. 

![6](../images/mongodb_20190308_6.jpg)


- **mongodb --help**를 통하여 명령어를 확인하면 **--dbpath arg** 형식으로 입력하면 dbpath를 설정할 수 있다. 아래와 같이 명령어를 입력해서 dbpath를 설정해주자(옵션에 들어가는 폴더는 직접 만들어주어야 한다). 
`mongod --dbpath C:\util\MongoDB\data\db`



- **wating for connections on port 27017** 이라고 출력되면 정상적으로 실행된 것이다. 27017은 포트번호이다.
![7](../images/mongodb_20190308_7.jpg)

- 브라우저를 실행시켜 localhost:27017을 입력하여 아래처럼 메시지가 출력하면 된다.
![8](../images/mongodb_20190308_8.jpg)

_ _ _



*출처 : 
- <https://javacpro.tistory.com/64> 참고