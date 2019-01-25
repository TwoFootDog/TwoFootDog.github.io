---
layout: post
title: "[Spring]Spring MVC JUnit 활용하기3(mockito를 활용한 Service, mapper(혹은 DAO) 테스트)"
description: Spring MVC 프로젝트에서 JUnit과 mockito를 활용한 Service, mapper(혹은 DAO) 단위테스트하기
image: '../images/강아지20.jpg'
category: 'SpringMVC'
tags : 
- IT
- Spring
- SpringMVC
- JUnit
- Mock
- mockito
- 테스트더블
- Test


twitter_text: 
introduction : Spring MVC 프로젝트에서 JUnit과 mockito를 활용하여 Service, mapper(혹은 DAO) 단위테스트 해보자.
---

**Spring MVC JUnit 활용하기** 에서 JUnit을 활용한 서비스 단위테스트를 진행하였고,
**Spring MVC Junit 활용하기2((MockMvc를 활용한 Controller 단위테스트)** 에서 JUnit과 MockMvc를 활용하여 컨트롤러 단위테스트를 진행하였다. 
이번에는 JUnit과 mockito 활용하여 서비스와 DB 단위테스트를 진행하도록 한다.



* * *



1. mockito & 테스트 더블이란?

**mockito는 JUnit위에서 동작하며 Mocking과 Verification을 도와주는 유닛 테스트를 위한 프레임워크다.** 
서비스와 DAO(또는 mapper)의 단위테스트 시 사용 가능하다.

mockito에 대한 자세한 설명에 앞서 **테스트 더블** 에 대해 알 필요가 있다.
**테스트 더블** 이란 테스트 시 실제 객체를 대신할 수 있는 객체를 의미한다. 종류로는 **Stub, Mock, Fake Object **등이 있다. 각각 다른 용도를 가지고 있기 때문에 어떤 테스트 더블을 언제 써야할지 알기 위해 서로 구분하는 것이 필요하다.



_ _ _


2. 테스트 더블 종류에 대한 간단 설명

각 종류에 대해 간단히 설명하면

1) **Stub**는 로직은 없고 단지 원하는 값을 반환한다. 테스트 시 "이 객체는 무조건 이 값을 반환한다" 라고 가정하고 사용할 수 있다. Stub는 작성은 쉽지만 Mocking Framework를 이용하는게 편하다. Mockito에서는 **when()** 메소드를 사용한다.


1-1) **when()** 은 특정 목 객체를 만들었다면 이 객체로부터 특정 조건을 지정할 수 있다.
```
    @Test
    public void testWhen() {
        TestService2 service = mock(TestService2.class);	// mock 객체 생성
        when(service.getAge(1)).thenReturn(2);				// getAge()함수 호출 시 2가 리턴되게끔 지정
        assertTrue(service.getAge(1) == 2);					// 정상
    }
```


_ _ _



1-2) **doThrow()** 는 예외를 던지고 싶을 때 사용한다.
```
    @Test
    public void testDoThrow() {
        TestService2 service = mock(TestService2.class);
        doThrow(new IllegalArgumentException()).when(service).getAge(1);
        assertTrue(service.getAge(1) == 2);
    }
```


_ _ _



1-3) **doNothing()**은 간혹 void로 선언된 메소드에 when()을 걸고 싶을 때 사용한다.(라고 썼지만 언제 써야 하는지 좀 의문이다. 앞으로 더 공부를 해보도록 하겠다.)
```
    @Test
    public void testDoNothing() {
        TestService2 service = mock(TestService2.class);
        doNothing().when(service).setAge(10);
        service.setAge(10);
    }

```



_ _ _



2) **Mock**은 "어떤 메소드가 호출될 것이다" 라는 행위에 대한 예상을 가지고 있다. 만약 그 예상대로 메소드가 호출되지 않을 경우 테스트는 실패한다. 이렇듯 Mock은 객체 사이의 행위(interaction)를 테스트하기 위해 사용한다. 식별할 수 있는 상태 변경이 없거나 반환값으로 확인할 수 없는 경우에 유용하다. 예를 들면 어떤 코드가 디스크에서 read 작업을 하는데 하나 이상의 디스크에서 read 작업을 수행하지 않도록 하려는 경우, read 작업을 수행하는 메소드가 한번만 호출되었는지 검증하기 위해 Mock을 사용할 수 있다. mockito에서는 **verify()** 메소드를 사용한다.

2-1)**verify()**는 해당 구문이 호출되었는지 체크한다. 단순 호출뿐 아니라 횟수나 타임아웃 시간까지 지정해서 체크해 볼 수 있다.
```
    @Test
    public void testVerify() {
        TestService2 service = mock(TestService2.class);        // mock 객체 생성
        BoardVO board = service.getBoardInfo(10L);         // mock 객체 함수 호출(argument 10L)

        verify(service).getBoardInfo(10L);                 // 해당 함수가 argument 10L로 호출된 적이 있는지 검증. 정상처리
        verify(service, times(1)).getBoardInfo(10L);        // 해당 함수가 argument 10L로 1번 호출된 적이 있는지 검증. 정상 처리
        verify(service, times(1)).getBoardInfo(anyLong());       // 해당 함수가 argument Long 타입으로 호출된 적이 있는지 검증. 정상 처리
        verify(service, atLeastOnce()).getBoardInfo(10L);                           // 해당 함수가 argument 10L로 최소 한번 호출된 적이 있는지 검증. 정상 처리
        verify(service, atMost(2)).getBoardInfo(anyLong());        // 해당 함수가 argument Long 타입으로 호출된 적이 최대 2번인지 검증. 정상 처리
        verify(service, timeout(100)).getBoardInfo(10L);                      // 해당 함수가 100 밀리초 이내에 호출디 되었는지 검증. 정상처리
        //verify(service, never()).getBoardInfo(anyLong());                              // 해당 함수가 argument Long 타입으로 호출된 적이 없는지 검증. 에러 처리
//        verify(service, never()).getBoardInfo(10L);                                    // 해당 함수가 argument 10L로 호출된 적이 없는지 검증. 에러 처리
        verify(service, never()).getBoardInfo(1L);                                  // 해당 함수가 argument 1L로 호출된 적이 없는지 검증. 정상 처리
    }
```



- - -


3) **Fake**는 Mocking Framework(예 mockito)를 사용하지 않는다. 실제 구현처럼 동작하게끔 간단히 구현한다. Fake는 테스트 수행 시 실제 구현객체가 너무 느리거나 네트워크를 통해 무언가 수행해야하는 등의 이유로 이용할 수 없을 경우에 사용할 수 있다. Fake는 대체로 실제 구현 객체를 개발한 사람이나 팀에 의해 생성되거나 유지보수 되기 때문에 직접 작성할 필요는 없다. (보통 익명 클래스로 특정 메소드를 간단히 Overriding 해서 이용하기도 한다)



- - -




3. mockito를 활용한 단위테스트 방법




1) pom.xml에 spring-test 및 JUnit 라이브러리를 추가한다.(maven repo에서 최신버전으로 적용한다.)
```
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
            <scope>test</scope>
        </dependency>
        <!-- mock -->
        <dependency>
            <groupId>org.mockito</groupId>
            <artifactId>mockito-all</artifactId>
            <version>1.10.19</version>
            <scope>test</scope>
        </dependency>
```





- - -





2) Test 대상인 클래스를 생성한다.(Service, Dao, Mapper 등)

src/main/java/com/test/service/TestService2.java
```
package com.test.service;

import com.test.dao.TestDao;
import com.test.domain.BoardVO;
import lombok.Setter;
import lombok.extern.log4j.Log4j;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
import org.springframework.util.StringUtils;

@Service
@Log4j
public class TestService2 {

    @Setter(onMethod_ = {@Autowired})
    private TestDao dao;

    BoardVO boardVO;
    int age;

    public BoardVO getBoardInfo(Long bno) {
        serviceBefore();
        boardVO = dao.getBoardInfo(bno);
        if (!StringUtils.isEmpty(boardVO)) {
            return boardVO;
        } else {
            throw new RuntimeException();
        }
    }

    public int getAge(int age) {
        this.age = age + 1;
        return age;
    }

    public void setAge(int age) {
        this.age = age + 2;
    }

    public void serviceBefore() {
        log.info("====================serviceBefore====================");
    }

    public void serviceAfter() {
        log.info("====================serviceAfter====================");
    }
}
```





src/main/java/com/test/dao/TestDao.java
```
package com.test.dao;

import com.test.domain.BoardVO;
import com.test.mapper.TestMapper;
import lombok.Setter;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Repository;

@Repository
public class TestDao {

    @Setter(onMethod_= {@Autowired})
    private TestMapper testMapper;

    BoardVO boardVO;

    public BoardVO getBoardInfo(Long bno) {

        boardVO = testMapper.getBoard(bno);

        return boardVO;
    }
}
```




src/main/java/com/test/mapper/TestDao.java
```
package com.test.mapper;

import com.test.domain.BoardVO;

public interface TestMapper {
    public BoardVO getBoard(Long bno);
}
```




src/main/java/com/test/domain/BoardVO.java
```
package com.test.domain;

import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

import java.util.Date;

@Data
@AllArgsConstructor
@NoArgsConstructor
public class BoardVO {
    private Long bno;
    private String title;
    private String content;
    private String writer;
    private Date registDate;
    private Date updateDate;
}
```




/src/main/resources/com/test/mapper/TestMapper.xml
```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.test.mapper.TestMapper">
  <select id="getBoard" resultType="com.test.domain.BoardVO">
    select
    bno,
    title,
    content,
    writer,
    registDate,
    updateDate
    FROM tbl_board
    WHERE BNO = #{bno}
  </select>
</mapper>
```




- - -





3) mockito를 활용하여 테스트 클래스를 작성 후 JUnit 테스트를 실행한다.(Alt + Shift + F10)
테스트 해볼 클래스 구조는 서비스(TestService2.java) -> Dao(TestDao.java) -> Mapper(TestMapper.java) 의 구조로 되어있으며, 
서비스의 입장에서 가장 중요한 것은 해당 서비스의 메소드 호출 시 Dao의 메소드를 정상 호출했는지 여부이다.
따라서 테스트는 서비스의 메소드 정상호출여부, 서비스 메소드 호출 시 Dao의 메소드 정상 호출여부, 서비스 메소드 호출 시 서비스의 다른 메소드 정상 호출여부를 체크하겠다.

3-1) 서비스의 메소드 정상 호출 여부 테스트.
**mock()으로 mock객체를 만들게 되면 해당 객체는 기능이 전혀 없는 껍데기 객체에 불가하다. 때문에 객체 내의 비즈니스 로직 테스트 등은 불가능하다.** 때문에 코드 상으로는 TestService2가 getBoardInfo() 메소드 호출 시 메소드 내에서 serviceBefore()가 호출되지만 mock 객체에서는 serviceBefore() 메소드가 호출되지 않기 때문에 verify()로 검증 이 불가능하다.(이 건에 대해서는 아래에서 좀 더 자세히 다루도록 하겠다.)
```
package com.test;


import com.test.dao.TestDao;
import com.test.domain.BoardVO;
import com.test.service.TestService2;
import lombok.extern.log4j.Log4j;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;
import org.springframework.test.context.web.WebAppConfiguration;

import java.util.Date;

import static junit.framework.TestCase.assertTrue;
import static org.mockito.Mockito.*;


@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration({"file:src/main/webapp/WEB-INF/spring-config/applicationContext.xml", "file:src/main/webapp/WEB-INF/spring-config/dispatcher-servlet.xml"})
@WebAppConfiguration
@Log4j
public class MockitoTests {

    @Test
    public void testService() {
        TestService2 service = mock(TestService2.class);	// mock 객체 생성

        BoardVO boardVO = service.getBoardInfo(10L);		// 메소드 호출
        verify(service).getBoardInfo(10L);					// 메소드 정상 호출여부 검증
    }

}

```


위 코드를 보면 `TestService2 service = mock(TestService2.class);`로 mock 객체를 생성하는데 **@Mock** 어노테이션을 활용하여 mock 객체를 생성할 수도 있다. 

```
    @Mock
    TestService2 service;
    @Before
    public void setUp() {
        MockitoAnnotations.initMocks(this);     // Mock으로 지정된 객체 생성
    }

    @Test
    public void testService() {
//        TestService2 service = mock(TestService2.class);
        BoardVO boardVO = service.getBoardInfo(10L);
        verify(service).getBoardInfo(10L);
    }
```




- - -



3-2) 서비스의 메소드 호출 시 서비스 내 Dao의 메소드 정상 호출여부 테스트. 
현재 서비스 내에 Dao가 @Autowired로 의존주입되어있다. 서비스(TestService2)의 메소드(getBoardInfo)를 호출하게 되면 서비스 내 의존주입된 Dao(TestDao)의 메소드(getBoardInfo) 가 정상 호출되는지 테스트 해볼 것이다.

```
    @Test
    public void testDao() {
        TestDao dao = mock(TestDao.class);          // mock 객체 생성
        TestService2 service = new TestService2();  // 실제 객체 생성
        service.setDao(dao);                        // mock 객체를 실제 객체에 주입

        when(dao.getBoardInfo(1L)).thenReturn(new BoardVO(1L, "title1", "content1", "writer1", new Date(), new Date()));    // 결과값 지정(stub)
        BoardVO boardVO = service.getBoardInfo(1L); // 서비스의 getBoardInfo() 메소드 호출
        verify(dao).getBoardInfo(1L);               // // mock객체 dao의 메소드인 getBoardInfo()가 정상 호출되었는지 검증
    }
```
3-1과 바뀐 내용은 Dao Mock 객체를 서비스에 주입하는 내용과 verify() 메소드 호출을 testService가 아닌 dao를 한다는 것이다(testService는 mock객체가 아닌 실제 객체이기 때문에 verify() 메소드를 사용할 수 없다.). 
또한 when() 메소드를 사용하여 return 값을 지정해 주었다. 만약 값을 지정해주지 않으면 `public BoardVO getBoardInfo(Long bno)` 메소드 내에서 `boardVO = dao.getBoardInfo(bno)` 실행을 하게 되는데, dao는 mock 객체이기 때문에 getBoardInfo에는 값이 넘어오지 않아 boardVO는 null값이 된다. 그런데 boardVO가 비어있으면 RuntimeException을 throw 하게끔 코딩이 되어있으므로(TestService2) RuntimeException이 발생한다. 
추가로 위 코드는 **@Mock, @InjectMocks** 어노테이션을 사용하여 아래와 같이 변경할 수 있다.

```
    @Mock
    TestDao dao;
    @InjectMocks
    TestService2 service;

    @Before
    public void setUp() {
        MockitoAnnotations.initMocks(this);     // Mock으로 지정된 객체 생성
    }
    
    @Test
    public void testDao() {
//        TestDao dao = mock(TestDao.class);
//        TestService2 service = new TestService2();
//        service.setDao(dao);

        when(dao.getBoardInfo(1L)).thenReturn(new BoardVO(1L, "title1", "content1", "writer1", new Date(), new Date()));
        BoardVO boardVO = service.getBoardInfo(1L);
        verify(service).getBoardInfo(1L);
        verify(dao).getBoardInfo(1L);
    }
```


- - -

3-3) 서비스의 메소드 호출 시 서비스의 다른 메소드 호출 여부 테스트.
서비스의 A메소드 호출 시 서비스의 다른 B메소드를 호출하게 되는 경우 해당 B메소드가 정상적으로 호출이 되는지 테스트를 해 볼 것이다. mock 객체를 생성하게 되면 껍데기만 구현이 되기 때문에 A메소드에서 B메소드를 호출을 하는 로직은 생성이 되지 않는다. **이럴 때에는 mock() 대신에 spy()를 써서 실제 객체를 만들어 테스트를 진행하면 된다.** spy()로 만든 객체는 실제 매소드를 호출하며 멤버가 올바르게 호출되었는지 확인하기가 쉽다. 
**서비스를 생성자를 통해(new TestService2())로 생성하지 않고 spy()를 통해 생성하는 이유는 생성자를 통해 만든 객체의 경우는 verify()를 사용할 수 없기 때문이다.**

```
    @Test
    public void testDaoSpy2() {
        TestService2 service = spy(TestService2.class);     // spy 객체 생성
        TestDao dao = mock(TestDao.class);                  // mock 객체 생성
        service.setDao(dao);                                // mock 객체를 spy 객체에 주입

        when(dao.getBoardInfo(10L)).thenReturn(new BoardVO(1L, "title1", "content1", "writer1", new Date(), new Date()));   // 결과값 지정(stub)
        BoardVO boardVO = service.getBoardInfo(10L);    // 서비스의 getBoardInfo() 메소드 호출
        verify(service).getBoardInfo(10L);              // 서비스의 실제 메소드인 getBoardInfo() 가 정상 호출되었는지 검증
        verify(service).serviceBefore();                     // 서비스의 실제 메소드인 serviceBefore() 가 정상 호출되었는지 검증
        verify(dao).getBoardInfo(10L);                  // mock객체 dao의 메소드인 getBoardInfo()가 정상 호출되었는지 검증
    }
```
위 코드는 **@Mock, @InjectMocks, @Spy** 어노테이션을 사용하여 아래와 같이 변경 가능하다
```
    @Mock
    TestDao dao;
    @Spy           // @Spy는 InjectMocks와 동시 사용 가능
    @InjectMocks
    TestService2 service;

    @Before
    public void setUp() {
        MockitoAnnotations.initMocks(this);     // Mock으로 지정된 객체 생성
    }
    
    @Test
    public void testDaoSpy2() {
//        TestService2 service = spy(TestService2.class);     // spy 객체 생성
//        TestDao dao = mock(TestDao.class);                  // mock 객체 생성
//        service.setDao(dao);                                // mock 객체를 spy 객체에 주입

        when(dao.getBoardInfo(10L)).thenReturn(new BoardVO(1L, "title1", "content1", "writer1", new Date(), new Date()));   // 결과값 지정(stub)
        BoardVO boardVO = service.getBoardInfo(10L);    // 서비스의 getBoardInfo() 메소드 호출
        verify(service).getBoardInfo(10L);              // 서비스의 실제 메소드인 getBoardInfo() 가 정상 호출되었는지 검증
        verify(service).serviceBefore();                     // 서비스의 실제 메소드인 serviceBefore() 가 정상 호출되었는지 검증
        verify(dao).getBoardInfo(10L);                  // mock객체 dao의 메소드인 getBoardInfo()가 정상 호출되었는지 검증
    }
```




5) 언제 Mock Object를 사용해야 할까?




- - -










*출처 : 
<https://jdm.kr/blog/222>
<http://www.jpstory.net/2013/07/26/know-your-test-doubles/>
<http://egloos.zum.com/kingori/v/4169398>
<https://jojoldu.tistory.com/239>참고
