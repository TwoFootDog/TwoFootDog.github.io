---
layout: post
title: "[Mybatis]There is no getter for property named 에러"
description: Mybatis 에러 원인 및 해결 방법
image: '../images/강아지9.jpg'
category: 'Mybatis'
tags : 
- IT
- Mybatis
- Spring
twitter_text: 
introduction: There is no getter for property named 에러
---
Spring5 프로젝트에서 table Update 소스 테스트 중 
**nested exception is org.apache.ibatis.reflection.ReflectionException: There is no getter for property named 'REPLY' in 'class org.zerock.domain.ReplyVO'**

에러가 발생을 하였다.
REPLY 변수에 대한 getter함수가 없다는 내용인데, 분명 소스에는 @Data로 getter함수를 만들어줬는데 에러가 발생하였다.


확인을 해보니 ReplyVO에 선언된 reply, rno 변수와 ReplyMapper.xml에 바인딩하는 #{REPLY}, #{BNO} 변수가 불일치해서 난 에러였다. 변수를 사용할 때 대소문자도 중요하니 유념해두도록 하자.


ReplyMapper.xml(수정 전)
```
    <update id="UPDATE">
        UPDATE TBL_REPLY
        SET REPLY = #{REPLY}, UPDATEDATE = SYSDATE()
        WHERE RNO = #{RNO}
    </update>
```




ReplyMapper.xml(수정 후)
```
    <update id="UPDATE">
        UPDATE TBL_REPLY
        SET REPLY = #{reply}, UPDATEDATE = SYSDATE()
        WHERE RNO = #{rno}
    </update>
```



ReplyMapper.java
```
package org.zerock.mapper;

import org.zerock.domain.ReplyVO;

public interface ReplyMapper {
    public int insert(ReplyVO replyVO);

    public ReplyVO read(Long bno);

    public int delete(Long bno);

    public int update(ReplyVO replyVO);
}

```


ReplyVO.java
```
package org.zerock.domain;

import lombok.Data;
import java.util.Date;

@Data
public class ReplyVO {
    private Long rno;
    private Long bno;
    private String reply;
    private String replyer;
    private Date replyDate;
    private Date updateDate;
}
```



ReplyMapperTests.java
```
package org.zerock.mapper;

import lombok.Setter;
import lombok.extern.log4j.Log4j;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;
import org.zerock.domain.ReplyVO;

import java.util.stream.IntStream;

@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration("file:src/main/webapp/WEB-INF/spring/root-context.xml")
@Log4j
public class ReplyMapperTests {

    @Setter(onMethod_ = @Autowired)
    private ReplyMapper replyMapper;

    private Long[] bnoArr = {6L, 7L, 8L, 9L, 10L};

    @Test
    public void testUpdate() {
        Long targetRno = 3002L;
        ReplyVO replyVO = replyMapper.read(3002L);
        replyVO.setReply("댓글테스트 수정본");
        int count = replyMapper.update(replyVO);
        log.info("update count : " + count);
    }
}
```




_ _ _






*출처 : 음슴
