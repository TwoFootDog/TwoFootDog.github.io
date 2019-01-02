---
layout: post
title: "[Spring]Spring MVC에서 ControllerAdvice를 활용한 예외처리(Exception Handling)"
description: Spring MVC 프로젝트에서 ControllerAdvice Annotation을 활용한 예외처리(Exception Handling)
image: '../images/강아지17.jpg'
category: 'SpringMVC'
tags : 
- IT
- Spring
- SpringMVC
- Annotation
- Exception
- ControllerAdvice

twitter_text: 
introduction : Spring MVC 프로젝트에서 ControllerAdvice Annotation을 활용하여 예외처리(Exception handling)를 전역처리해보자.
---

Spring 프로젝트를 진행하다 보면 Exception 을 처리해야 하는 경우가 있다.
Exception을 처리하는 방식은 3가지가 있다.
1) try/catch 문을 활용한 메소드 단위의 Exception 처리
2) @ExceptionHandler를 활용한 Controller단위의 Exception 처리
3) @ControllerAdvice를 활용한 전역 Exception 처리

이중에서 3)@ControllerAdvice를 활용한 Exception 처리에 대해 설명을 해보고자 한다.


_ _ _



1) src/main/java/임의의패키지에 예외 처리 시 사용할 ValidException클래스를 생성한다. 해당 클래스는 RuntimeException을 상속한 예외 클래스이다.
```
package com.commons.exception;


import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

@AllArgsConstructor
@NoArgsConstructor
@Data
public class ValidException extends RuntimeException{
    private String ans_cd;
    private String ans_msg;
}


```








_ _ _


2) src/main/java/임의의패키지에 GlobalExceptionHandler 클래스를 생성한다. 해당 클래스는 전체 클래스에서 발생하는 예외처리를 일괄적으로 처리하는 클래스이다.


```
package com.commons.exception;


import lombok.extern.log4j.Log4j;
import org.springframework.http.HttpHeaders;
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.ControllerAdvice;
import org.springframework.web.bind.annotation.ExceptionHandler;
import org.springframework.web.bind.annotation.ResponseBody;

@ControllerAdvice
@ResponseBody
@Log4j
public class GlobalExceptionHandler{

    @ExceptionHandler(value = ValidException.class)
    public ResponseEntity<Object> handleValidException(ValidException e){

        HttpHeaders responseHeader = new HttpHeaders();
        responseHeader.add("ans_cd", e.getAns_cd());


        log.error("business error occur. ans_cd : " + e.getAns_cd() + ", error message : " + e.getAns_msg());

        return new ResponseEntity<>(responseHeader, HttpStatus.OK);
    }
}


```
**@ControllerAdvice**
 - Controller를 보조하는 클래스에 사용하는 어노테이션으로, Controller에서 쓰이는 공통 기능들을 모듈화하여 전역으로 사용 가능하다.

**ResponseBody**
- 리턴되는 값이 View를 통한 리턴이 아닌, HTTP Response Body에 쓰이게 된다. ControllerAdvice + ResponseBody 대신 RestControllerAdvice를 써도 무방하다.

**ExceptionHandler**
- Controller 메소드에 사용하는 어노테이션으로, 어노테이션 인자로 전달된 예외를 처리한다. 

즉 Controller나 Service 등에서 ExceptionHandler에 전달된 예외(ValidException) 발생 시 handleValidException함수를 호출하게 된다. 함수 내에서는 예외 발생 시 전달된 ans_cd값을 header에 셋팅한 후 ResponseEntity 형태로 결과를 리턴되게 된다.  


_ _ _


3) applicationContext.xml(혹은 root-context.xml)에 ControllerAdvice가 선언된 패키지를 추가하여 bean으로 등록될 수 있게 한다.

```
<context:component-scan base-package="com.commons.exception" />
```




_ _ _


4) 예외 발생 조건이 충족되는 경우 **"throw new 예외명"** 으로 예외를 발생시킨다. 예외를 발생시키면 @ControllerAdvice가 선언된 클래스로 가서 해당 예외가 선언된 함수가 수행된다.

```
package com.module.service;


@Service
@Log4j
public class Service1 {

    private OutputVO outputVO;
    private HttpHeaders Headers;

    public ResponseEntity<OutputVO> function1(HttpServletRequest request, InputVO inputvo) {

        if (예외발생조건) {
            throw new ValidException("9999", "에러입니다.");
        }
        return new ResponseEntity<OutputVO>(outputVO, Headers, HttpStatus.OK); // 예외 발생 시 실행 안됨

    }

}
```


*출처 : <http://jijs.tistory.com/entry/spring-%EC%84%A4%EC%A0%95-xml%EA%B3%BC-%EC%86%8C%EC%8A%A4%EC%BD%94%EB%93%9C%EC%97%90%EC%84%9C-properties-%EC%82%AC%EC%9A%A9%ED%95%98%EA%B8%B0> 참고
