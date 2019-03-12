---
layout: post
title: "[Spring]POST 방식으로 전달된 JSON 데이터 처리하기"
description: 
image: '../images/강아지36.jpg'
category: 'SpringMVC'
tags : 
- IT
- Spring
- SpringMVC

twitter_text: 
introduction : Spring에서 POST 방식으로 전달된 JSON 데이터 값을 추출해보도록 하자
---

Spring Project 를 수행하다가 Parameter나 Header값이 아닌 POST방식으로 전송된 body 데이터("application/json" 타입)를 추출해야 할 필요가 생겼다. 
처음에는 쉽게 생각하여 HttpServletRequest를 활용하여 getAttribute() 함수를 사용하거나 그 외 여러 함수를 사용해 보았지만 소용이 없었다.
그래서 인터넷을 찾아본 결과 HttpServletRequest의 getReader() 함수나 getInputStream() 함수를 사용하면 추출을 할 수 있다고 하여 사용해 보았지만, 아래와 같은 메시지가 발생하며 오류 처리되었다.

![](../images/post_json_20190312.jpg)

위와 같은 메시지가 발생하는 원인은, HttpServletRequest의 InputStream은 한번 읽으면 다시 읽을 수 없다(톰캣 개발자들이 막아놨음). 

하지만 위와 같은 문제에 대한 해결책이 있다. 우선 Wrapper 객체를 하나 만들어서 일단 InputStream을 읽어서 내 맘대로 이것저것 작업한 뒤, 다른 곳에서 InputStream을 다시 읽으려고 시도하는 경우 이미 읽었던 데이터로 다시 InputStream을 생성해 돌려주도록 만드는 방법이 있다. 

만드는 방법은 아래와 같다.




_ _ _





### [pom.xml 의존성 추가]
재정의에 필요한 메소드 사용을 위하여 pom.xml에 의존성을 추가한다

**pom.xml**
```
        <!-- JSONObject 사용-->
        <dependency>
            <groupId>org.json</groupId>
            <artifactId>json</artifactId>
            <version>20180813</version>
        </dependency>
        <!-- IOUtils 사용-->
        <dependency>
            <groupId>commons-io</groupId>
            <artifactId>commons-io</artifactId>
            <version>2.6</version>
        </dependency>
        <!-- isBlank 사용 -->
        <dependency>
            <groupId>org.apache.commons</groupId>
            <artifactId>commons-lang3</artifactId>
            <version>3.0</version>
        </dependency>
        <!-- URLCodec 사용-->
        <dependency>
            <groupId>commons-codec</groupId>
            <artifactId>commons-codec</artifactId>
            <version>1.4</version>
        </dependency>
```



_ _ _




### [HttpServletRequestWrapper 클래스 확장]
getInputStream() 메서드를 override한 wrapper를 만들어보자. 그러기 위해선 javax.servlet.http 패키지에 HttpServletRequest를 래핑할때 스기 위해 미리 준비해둔 **HttpServletRequestWrapper**라는 클래스를 확장(재정의)하여 새로운 Wrapper 클래스를 만들면 된다.

**RereadableRequestWrapper.java**
```
package com.commons.util;

import org.apache.commons.codec.DecoderException;
import org.apache.commons.codec.net.URLCodec;
import org.apache.commons.io.IOUtils;
import org.apache.commons.lang3.StringUtils;

import javax.servlet.ReadListener;
import javax.servlet.ServletInputStream;
import javax.servlet.ServletRequest;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletRequestWrapper;
import java.io.*;
import java.nio.charset.Charset;
import java.nio.charset.StandardCharsets;
import java.util.*;

public class RereadableRequestWrapper extends HttpServletRequestWrapper {

    private boolean parametersParsed = false;

    private final Charset encoding;
    private byte[] rawData;
    private final Map<String, ArrayList<String>> parameters = new LinkedHashMap<String, ArrayList<String>>();
    ByteChunk tmpName = new ByteChunk();
    ByteChunk tmpValue = new ByteChunk();

    private class ByteChunk {

        private byte[] buff;
        private int start = 0;
        private int end;

        public void setByteChunk(byte[] b, int off, int len) {
            buff = b;
            start = off;
            end = start + len;
        }

        public byte[] getBytes() {
            return buff;
        }

        public int getStart() {
            return start;
        }

        public int getEnd() {
            return end;
        }

        public void recycle() {
            buff = null;
            start = 0;
            end = 0;
        }
    }


    public RereadableRequestWrapper(HttpServletRequest request) throws IOException {
        super(request);

        String characterEncoding = request.getCharacterEncoding();
        if (StringUtils.isBlank(characterEncoding)) {
            characterEncoding = StandardCharsets.UTF_8.name();
        }
        this.encoding = Charset.forName(characterEncoding);

        // Convert InputStream data to byte array and store it to this wrapper instance.
        try {
            InputStream inputStream = request.getInputStream();
            this.rawData = IOUtils.toByteArray(inputStream);
        } catch (IOException e) {
            throw e;
        }
    }

    @Override
    public ServletInputStream getInputStream() throws IOException {
        final ByteArrayInputStream byteArrayInputStream = new ByteArrayInputStream(this.rawData);
        ServletInputStream servletInputStream = new ServletInputStream() {
            @Override
            public boolean isFinished() {
                return false;
            }

            @Override
            public boolean isReady() {
                return false;
            }

            @Override
            public void setReadListener(ReadListener readListener) {

            }

            public int read() throws IOException {
                return byteArrayInputStream.read();
            }
        };
        return servletInputStream;
    }

    @Override
    public BufferedReader getReader() throws IOException {
        return new BufferedReader(new InputStreamReader(this.getInputStream(), this.encoding));
    }

    @Override
    public ServletRequest getRequest() {
        return super.getRequest();
    }
```





_ _ _






### [Servlet Filter에서부터 Wrapper 클래스로 전환]
그런데 만약 Servlet Filter가 아닌 Spring Interceptor에서 이 Wrapper 클래스를 사용할 예정이라면, 추가적인 고려가 필요하다. Spring Interceptor를 만들면 context.xml에서 등록을 할텐데, 이는 곧 Spring의 DispatcherServlet에서 Interceptor를 핸들링한다는 뜻이다. 만약 Interceptor 내에서 Wrapper를 만들어서 preHandle()에 넘겨주게 되면 이후 Spring이 데이터를 바인딩할 때 결국 Stream이 닫혔다는 메시지를 다시 만나게 될 수 있다. 그 원인은 Interceptor가 DispatcherServlet의 doDispatch() 메서드 내에서 열심히 loop를 들면서 실행된 뒤에 다음 구문에서 데이터 바인딩을 하러 가기 때문이다. 다시 말해서 Interceptor 내에서 preHandle()로 넘겨준 request 객체가 데이터 바인딩 작업을 하러 갈때는 call by value에 따라 이미 사라지고 없어진 상황이다.

따라서 DispatcherSErvlet으로 가기 전인 Servlet Filter에서부터 wrapper 클래스로 전환해주어야 정상 동작한다. Entry point가 되는 적절한 Filter속에서 Wrapper로 전환하는 작업을 하고 doFilter() 메서드에는 Wrapping한 request를 넘겨주면 Interceptor에서도 Wrapper된 request 객체를 받아와서 잘 사용할 수 있다.

**RequestFilter.java**
```
package com.commons.util;

import javax.servlet.FilterChain;
import javax.servlet.ServletException;
import javax.servlet.ServletRequest;
import javax.servlet.ServletResponse;
import javax.servlet.http.HttpServletRequest;
import java.io.IOException;

public class RequestFilter implements javax.servlet.Filter {
    @Override
    public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws IOException, ServletException {
        RereadableRequestWrapper rereadableRequestWrapper = new RereadableRequestWrapper((HttpServletRequest)request);
        chain.doFilter(rereadableRequestWrapper, response);
    }
}


```



_ _ _




### [web.xml 수정]
추가한 RequestFilter를 web.xml에 등록해준다.

**web.xml**
```
    <!-- request body를 받기 위해 추가 start-->
    <filter>
        <filter-name>requestFilter</filter-name>
        <filter-class>com.commons.util.RequestFilter</filter-class>
    </filter>
    <filter-mapping>
        <filter-name>requestFilter</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
    <!-- end-->
```


_ _ _




### [Json형태의 HttpServletRequest를 String 형태로 return]
추가적인 작업이지만 Json 형태의 HttpServletRequest를 JSONObject 형태로 return 해주는 메서드도 정의해 보았다. 

**JsonUtils.java**
```
package com.commons.util;

import lombok.extern.log4j.Log4j;
import org.json.JSONObject;

import javax.servlet.http.HttpServletRequest;
import java.io.BufferedReader;

@Log4j
public class JsonUtils {

    public JsonUtils() {
    }
    // json 형식으로 유입된 HttpServletRequest를 string 형태로 return
    public  JSONObject readJSONStringFromRequestBody(HttpServletRequest request){
        StringBuffer json = new StringBuffer();
        String line = null;

        try {
            BufferedReader reader = request.getReader();
            while((line = reader.readLine()) != null) {
                json.append(line);
            }

        }catch(Exception e) {
            log.info("Error reading JSON string: " + e.toString());
        }

        JSONObject jObj = new JSONObject(json);
        return jObj;
    }
}


```




_ _ _





### [Json 데이터 추출]
위에서 재정의한 클래스 및 매소드를 활용하여 POST방식으로 전송된 application/json 타입의 데이터 body를 추출한다. 아래 코드는 json 데이터를 CustomeHeaderVO 타입의 Object로 추출하는 방법이다.

**입력된 JSON 데이터**
```
{
    "header": {
        "uuId": "ABCDEFDFADFDADFADFF",
        "sendDy": "20190227",
        "sendTm": "170000",
        "ansCd": "",
        "message" : ""
    },
    "body": [{
        "grid": 1
    }],
    "totalBodyCnt": 1
}

```


**입력된 JSON 데이터에서 header를 추출하는 코드(header의 타입은 자체 정의한 CustomizeHeaderVO 클래스 타입이다)**
```
        JsonUtils jsonUtils = new JsonUtils();  // 자체 정의한 jsonUtils 객체 생성
        JSONObject jObj = jsonUtils.readJSONStringFromRequestBody(request); // HttpServletRequest를 JSONObject 형태로 변환


        ObjectMapper objectMapper = new ObjectMapper();
        try {
            CustomizeHeaderVO requestCustomHeader = objectMapper.readValue(jObj.get("header").toString(), CustomizeHeaderVO.class);   // JSONObject에 포함된 CustomerHeader를 추출
        } catch (IOException e1) {
            e1.printStackTrace();
        }

        /* TEST 용 */
        log.info("json header-------------" + requestCustomHeader);
```



_ _ _



*출처 : 
- <https://meetup.toast.com/posts/44> 참고