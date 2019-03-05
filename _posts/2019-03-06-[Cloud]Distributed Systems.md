---
layout: post
title: "[Cloud]분산시스템(Distributed System)"
description: 
image: '../images/강아지31.jpg'
category: 'Cloud'
tags : 
- IT
- Cloud

twitter_text: 
introduction : 분산시스템(Distributed System)의 정의에 대해 알아보자
---

### [분산 시스템의 이해]

분산시스템의 정의
- 각 독립된 컴퓨터를 모아서 하나의 시스템으로 사용하는 것
- 초창기에는 no shared memory, no shared clock
- 각 컴퓨터는 각자의 operating system을 가지고 있음


분산시스템의 장점
- 커뮤니케이션과 자원(resource)을 sharing하기 위함
- 경제성(price와 performance의 적절한 조화가 필요함)
- 신뢰성(Reliability)과 확장성(Scalability)
- IT기술을 요구하는 비즈니스가 얼마나 성장할지 모르기 때문에 초기 리소스 셋팅이 어렵지만 분산시스템은 확장성이 뛰어나므로 리소스 셋팅이 용이

분산시스템의 단점
- 서로다른 os, 하드웨어의 통신을 위한 어플리케이션 등을 추가로 만들어야 하는 부담감
- 네트워크 연결이 필수
- security & privacy 문제

분산시스템 구축 시 개발자 고려사항 & 유저 투명성(Transparency)
- Access : 리소스에 어떻게 접근해야 하는지. 데이터가 어떻게 표현되어야 하는지
- Location : 사용하는 리소스의 위치를 알 필요 없음
- migration : 리소스가 한곳에서 다른곳으로 옮겨진 것을 알 필요 없음
- Relocation : 컴퓨팅을 쓰고 있는 중간에 자원에 문제가 생길 경우 수행하는 프로그램을 다른 컴퓨터에서 수행
- Replication : 리소스가 여러 유저에 의해서 공유되어져야 함(복제)
- Concurrency : 성능을 높이기 위해 한 프로그램이 cpu 여러개를 돌려야하는 상황이 오는데 유저는 몰라도 된다
- Failure : 자원이 잘못되었을 때, 오류가 났을 때 recovery를 해야하는데 그 과정을 유저에게 보일 필요가 없음
- Persistence : 소프트웨어가 메모리에 있거나 디스크에 있거나 상관없어야 함. 그렇지만 언제나 내가 접근할 수 있어야 함

분산시스템 하드웨어 컨셉
- 각 cpu별 개인 메모리 사용 or 공유 메모리 사용
- 버스(bus) 기반 or 스위치(switch) 기반 메모리 공유
- crossbar switch -> omega switching network(2x2 switch)
- Grid : 모든 버스가 연결되어 있음
- Hypercube : 여덟개의 시스템을 하나의 그룹을 구성


분산컴퓨터 모델
- minicomputer model : 각자 가진 로컬머신 사용. 가장 기본적인 네트워크 사용
- workstation model : 프로세싱이 마이그레이션 될수 있음
- Client-Server model : World Wide Web. 유저는 pc를 가지고 server에 접근
- processor pool model : 터미널 이용. 보이지 않는 여러개의 프로세스 pool을 이용해서 task를 수행

Uniprocessor Operating System
- 하나의 os가 리소스 매니저 역할을 함
- Uniprocessor os 구조
	- monolithic : 한개의 커널에 모든 기능을 넣음(ms-dos)
	- layered design : 기능을 여러개의 layer로 나눠서 각 layer가 여러 서비스를 사용할 수 있도록 함. 필요한 layer까지만 접근 가능
	- microkernel : 작은 커널을 만들어서 부팅 시간을 줄이고 user-level의 서버를 만들자

Distributed Operating System은 과연 무엇인가?
- 분산되어있는 자원을 access해야 하기 때문에, 유저에게 seamlessly and transparently 필요
- 그렇지만 유저에게는 한개의 시스템을 사용하는 것처럼 보여야 함
- transparency를 제공해야 함
- 유저에게는 보이지 않는 하나의 싱글 프로세스가 제공되어야 함

_ _ _





*출처 : 
- **클라우드 컴퓨팅 핵심기술 요소의 이해** 사내교육 참고