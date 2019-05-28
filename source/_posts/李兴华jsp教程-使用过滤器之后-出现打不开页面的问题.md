---
title: '李兴华jsp教程-使用过滤器之后,出现打不开页面的问题'
date: 2013-05-02 09:13:30
categories: Java
tags:
comments:
---

过滤器是依照李兴华的一本JSP书籍写的.,应用到MVC设计模式中出现了打不开页面的问题,.经过百度知道的提问,得到了解决,.

http://zhidao.baidu.com/question/546458767?quesup2&oldq=1

原来的代码

```java
package com.zkx.filter;
import java.io.*;
import javax.servlet.*;
public class EncodingFilter implements Filter{
//private String charSet;
public void init(FilterConfig config)throws ServletException{
//this.charSet = config.getInitParameter("charset"); //获取配置文件中设置的编码格式
}
public void doFilter(ServletRequest request,ServletResponse response, FilterChain chain)
throws IOException ,ServletException{
request.setCharacterEncoding("GBK");
}
public void destroy(){
}
}
-----------------------------------------------
配置文件
<filter>
<filter-name>encoding</filter-name>
<filter-class>com.zkx.filter.EncodingFilter</filter-class>
<init-param>
<param-name>charSet</param-name>
<param-value>GBK</param-value>
</init-param>
</filter>
<filter-mapping>
<filter-name>encoding</filter-name>
<url-pattern>/*</url-pattern>
</filter-mapping>
---------------------------------------------
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

```java
将doFilter改为如下解决问题
    public void doFilter(ServletRequest request,ServletResponse response, FilterChain chain)
    {
        try{
            request.setCharacterEncoding("GBK");          //增加了chain的doFilter 的方法.
            chain.doFilter(request, response);
        
        response.setCharacterEncoding("GBK");
        }catch(Exception e){
            e.printStackTrace();
        }
        
    }
顺利解决问题            request.setCharacterEncoding("GBK");          //增加了chain的doFilter 的方法.
            chain.doFilter(request, response);
        
        response.setCharacterEncoding("GBK");
        }catch(Exception e){
            e.printStackTrace();
        }
        
    }
顺利解决问题
```