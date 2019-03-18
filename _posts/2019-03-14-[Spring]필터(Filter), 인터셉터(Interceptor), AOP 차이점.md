---
layout: post
title: "[Spring]필터(Filter), 인터셉터(Interceptor), AOP 차이점"
description: 
image: '../images/강아지44.jpg'
category: 'SpringMVC'
tags : 
- IT
- Spring
- SpringMVC
- JAVA

twitter_text: 
introduction : 스프링에서 사용되는 Filter, Intercepter, AOP 의 차이점에 대해 알아보자.
---

스프링에서 사용되는 Filter, Inteceptor, AOP 세가지 모두 무슨 행동을 하기 전에 먼저 실행하거나, 실행한 후 추가적인 행동을 할 때 사용되는 기능이다. 그렇다면 이 세가지의 차이점에 대해 알아보자.


_ _ _

### [Filter, Interceptor, AOP의 흐름]

![](../images/filter_aop_20190313.jpg)
- Filter와 Interceptor는 Servlet 단위에서 실행된다. 반면 AOP는 메소드 앞에서 Proxy 패턴으로 실행된다. 실행순서는 Filter -> Interceptor -> AOP -> Interceptor -> Filter 순이다. 상세 순서는 다음과 같다.
- 필터를 웹 컨테이너 내에 생성한 후 초기화 시 init()이 호출된다. 그리고 doFilter가 호출된다.
- 컨트롤러에 들어가기 전에 preHandler()가 실행된다.
- 컨트롤러에 나와 postHandler(), afterCompletion(), doFilter() 순으로 실행된다.
	- 컨트롤러의 메서드 처리가 끝나 return 되고 화면에 띄워주는 처리가 되기 직전에 preHandler()가 호출된다. 
- 서블릿 종료시 Filter.destroy()가 실행된다.

_ _ _



### [필터(Filter)]

말 그대로 요청과 응답을 거른 뒤 정제하는 역할을 한다. 

서블릿 필터는 DispatcherServlet 이전에 실행이 되는데 필터가 동작하도록 지정된 자원의 앞단에서 요청내용을 변경하거나, 여러가지 체크를 수행할 수 있다. 또한 자원의 처리가 끝난 후 응답내용에 대해서도 변경 처리할 수 있다. **보통 web.xml에 등록하고, 일반적으로 인코딩처리, XSS방어등의 요청에 대한 처리로 사용된다.** (자세한 내용은 [필터란무엇인가](https://twofootdog.github.io/Spring-%ED%95%84%ED%84%B0(Filter)%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%B8%EA%B0%80/)참고 )

- 필터의 실행 메서드
	- init() : 필터 인스턴스 초기화
	- doFilter() : 전/후처리
	- destory() : 필터 인스턴스 종료



_ _ _


### [인터셉터(Interceptor)]

요청/응답을 가로채는 역할을 한다. 필터는 스프링 컨텍스트 외부에 존재하여 스프링과 무관한 자원에 대해 동작한다. 하지만 인터셉터는 스프링의 DispatcherServlet이 컨트롤러를 호출하기 전, 후로 끼어들기 때문에 스프링 영역에서 Controller에 대한 요청과 응답에 대해 처리한다. 또한 스프링의 모든 빈 객체에 접근할 수 있다.

**인터셉터는 여러개를 사용할 수 있고 로그인 처리, 권한체크, 프로그램 실행시간 계산작업, 로그확인 등의 업무처리에 활용된다.**
- 인터셉터 실행 메서드
	- public boolean preHandler(HttpServletRequest request, HttpSErvletResponse response, Object handler) : 컨트롤러 메서드가 실행되기 전 실행. request, response, handler 등의 매개변수를 이용가능한데 우리가 아는 HttpSErvletREquest, HttpServletResponse이고 나머지 하나는 preHandler()메서드를 수행하고 수행될 컨트롤러 메서드에 대한 정보를 담고 있는 handle이다)
	- void postHandler(HttpServletRequest request, HttpServletResponse, Object handler, ModelAndView modelAndView) : 컨트롤러 메서드 처리가 끝나 return 되고 화면을 띄워주는 처리가 되기 직전에 호출된다. ModelAndView 객체에 컨트롤러에서 전달해 온 Model 객체가 전달됨으로 컨트롤러에서 작업 후 postHandler()에서 작업할 것이 있다면 ModelAndView를 이용하면 된다.
	- void afterCompletion() : 컨트롤러가 수행되고 화면처리까지 끝난 뒤 호출된다.





_ _ _


### [AOP]

OOP를 보완하기 위해 나온 개념. 객체지향의 프로그램을 했을 때 중복을 줄일 수 없는 부분을 줄이기 위해 종단면(관점)에서 바라보고 처리한다. **주로 '로깅', '트랜잭션', '에러처리' 등 비즈니스 단의 메서드에서 조금 더 세밀하게 조정하고 싶을 때 사용한다.** 인터셉터나 필터와는 달리 메서드 전후의 지점에서 자유롭게 설정 가능하다. 또한 인터셉터나 필터는 주소로 대상을 구분해서 걸러내야 하는 반면, AOP는 주소, 파라미터, 애노테이션 등 다양한 방법으로 대상을 지정할 수 있다. 

AOP의 Advice와 HandlerInterceptor의 가장 큰 차이는 파라미터의 차이다. Advice의 경우 JoinPoint나 ProceedingJointPoint 등을 활용해서 호출한다. 반면 HandlerInterceptor는 Filter와 유사하게 HttpServletRequest/HttpServletResponse를 파라미터로 사용한다

- AOP의 포인트컷(PointCut)
	- @Before : 대상 메서드의 수행 전
	- @After : 대상 메서드의 수행 후
	- @Around : 대상 메서드의 수행 전/후
	- @After-returning : 대상 메서드의 정상적인 수행 후
	- @After-throwing : 예외 발생 후





_ _ _




*출처 : 
- <https://goddaehee.tistory.com/154> 
- <https://rongscodinghistory.tistory.com/2> 참고