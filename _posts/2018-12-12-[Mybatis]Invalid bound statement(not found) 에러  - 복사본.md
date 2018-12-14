---
layout: post
title: "[Mybatis]invalid bound statement (not found) 에러"
description: Mybatis 에러 원인 및 해결 방법
image: '../images/강아지7.jpg'
category: 'Mybatis'
tags : 
- IT
- IntelliJ
- Mybatis
- Spring
twitter_text: 
introduction: invalid bound statement (not found) 에러
---
Spring 프로젝트 중 Mybatis를 활용하여 DB에 접근하려 할 때
**org.apache.ibatis.binding.BindingException: Invalid bound statement (not found):** 에러가 발생하는 경우가 있다.

해당 에러가 발생하는 경우는 보통
1) Mapper 인터페이스와 Mapper xml에 오타가 있는 경우. Mapper 인터페이스의 함수와 Mapper xml의 id가 일치해야 하는데 오타가 나서 불일치하는 경우 에러가 발생할 수 있다.
2) Mapper xml 경로가 잘못된 경우.


1)의 경우는 눈에 금방 확인이 되기 때문에 쉽게 수정이 가능하지만, 2)의 경우 고생을 좀 한적이 있는데...
보통 Mapper xml의 경우 main/resources/ 밑에 디렉토리를 생성해서 만드는데
디렉토리를 일일이 하나씩 만든게 아니라(예를들면 com 디렉토리를 만들고 그 밑에 common디렉토리를 만들고 그 밑에 mapper 디렉토리를 만든 후 그 밑에 mapper.xml 생성), 디렉토리 명을 com.common.mapper로 만들고 그 밑에 mapper를 만드니깐 디렉토리 명 자체를 **com.common.mapper**로 인식하여 에러가 mapper의 정확한 위치를 찾지 못해 에러가 발생하였다.


때문에 resources에서 디렉토리를 만들 때에는 한개의 디렉토리씩 만들어주도록 하자.






_ _ _









*출처 : <http://blog.woniper.net/247>
