















# 开发学习

# **1.如何集成代码jar包**

1. 选择你的springboot项目 点击右上方的maven ，进入你的项目进入Lifecycle,双击package 

![image-20210917160316082](C:\Users\liumeng\AppData\Roaming\Typora\typora-user-images\image-20210917160316082.png)



2. 地下页面假如显示   Bulid Sucess 字样 就说明你到处jar包成功。显示false则有可能是你的驱动没有安装好

   

   ![image-20210917160412304](C:\Users\liumeng\AppData\Roaming\Typora\typora-user-images\image-20210917160412304.png)

可以自行百度搜索 一般就是缺少如下代码

```xml
<plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-resources-plugin</artifactId>
                <version>2.6</version>
            </plugin>
```

3.然后就会再target上生成jar包

![image-20210917160825282](C:\Users\liumeng\AppData\Roaming\Typora\typora-user-images\image-20210917160825282.png)

当然 你的本地code包下也会有jar包 

![image-20210917160858041](C:\Users\liumeng\AppData\Roaming\Typora\typora-user-images\image-20210917160858041.png)





4. 下一步就是用powershell 命令行进行运行

   

![image-20210917161225214](C:\Users\liumeng\AppData\Roaming\Typora\typora-user-images\image-20210917161225214.png)

此时我们要用powershell服务器 这里的shell还没怎么了解 日后再进一步深入挖掘其底层原理

然后一步一步看到spring字样的时候就是运行成功，这时我们输出localhost：8080/  即可



# 2 . 权限管理



抛出问题： 在系统中 ， 我们会遇到管理员和客户所进入到不同的页面 那么我们是如何处理这些权限的呢？



目前就学习到狂神说的视频而言 我们所运用的是拦截器这一层次而言的 还没有更加深入的运行到数据库的，日后再收集有关于此方面的知识技术。



样例：

1.登录成功之后 ，我们会设置一个session值 然后用其接受username属性 

![image-20211017213507053](C:\Users\liumeng\AppData\Roaming\Typora\typora-user-images\image-20211017213507053.png)



接在我在拦截器这儿或者session 所获得的 username 这里也就是loginUser   如果输出用户名没有值 就转发到首页 （不放行）

![image-20211017213735665](C:\Users\liumeng\AppData\Roaming\Typora\typora-user-images\image-20211017213735665.png)



配置的拦截器所拦截的请求

![image-20211017214123374](C:\Users\liumeng\AppData\Roaming\Typora\typora-user-images\image-20211017214123374.png)

# 3.页面国际化

1.首页配置：

a：注意点 所有页面的静态资源都需要用thymeleaf接管

b：url: @{}

```java
<link th:href="@{/css/bootstrap.min.css}" rel="stylesheet">
	<!-- Custom styles for this template -->
<link th:href="@{/css/signin.css}" rel="stylesheet">
```



2.页面国际化：



```java
<button class="btn btn-lg btn-primary btn-block" type="submit" >[[#{login.btn}]]</button>
			<p class="mt-5 mb-3 text-muted">© 2017-2018</p>
			<a class="btn btn-sm" th:href="@{/index.html(l='cn_CN')}">中文</a>
			<a class="btn btn-sm" th:href="@{/index.html(l='en_US')}">English</a>
	
```



a: 我们需要配置i18n文件

![image-20210923221851940](C:\Users\liumeng\AppData\Roaming\Typora\typora-user-images\image-20210923221851940.png)

b: 我们如果需要在项目中进行按钮自动切换，我们需要定义一个组件LocaleResolver

```java
package com.hue.stu.config;

import org.springframework.web.servlet.LocaleResolver;
import org.thymeleaf.util.StringUtils;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.util.Locale;

public class MyLocaleResolver implements LocaleResolver {
    @Override
    public Locale resolveLocale(HttpServletRequest httpServletRequest) {
       //获取请求中的参数链接
        String language = httpServletRequest.getParameter("l");
        //如果没有就使用默认的
        Locale locale =Locale.getDefault();
      //如果请求的链接携带了请求参数
        if(!StringUtils.isEmpty(language))
        {
            String[] spilt = language.split("_");
            locale = new Locale(spilt[0], spilt[1]); //逗号表达式
        }
        return locale;
    }

    @Override
    public void setLocale(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse, Locale locale) {

    }
}

```



c:记得将自己写的组件配置到spring容器 @Bean

```java
 //自定义的国际化组件就生效了
    @Bean
    public LocaleResolver localeResolver()
    {
        return new MyLocaleResolver();
    }
```



d：#{}

```java
<body class="text-center">
		<form class="form-signin" action="dashboard.html">
			<img class="mb-4" th:src="@{/img/bootstrap-solid.svg}" alt="" width="72" height="72">
			<h1 class="h3 mb-3 font-weight-normal" th:text="#{login.tip}">Please sign in</h1>
			<label class="sr-only"th:text="#{login.username}">Username</label>
			<input type="text" class="form-control" th:placeholder="#{login.username}" required="" autofocus="">
			<label class="sr-only" th:text="#{login.password}">Password</label>
			<input type="password" class="form-control" th:placeholder="#{login.password}" required="">
			<div class="checkbox mb-3">
				<label>
          <input type="checkbox" value="remember-me" >[[#{login.remember}]]
        </label>
			</div>
			<button class="btn btn-lg btn-primary btn-block" type="submit" >[[#{login.btn}]]</button>
```



重点：thymeleaf模块

![image-20210923213936890](C:\Users\liumeng\AppData\Roaming\Typora\typora-user-images\image-20210923213936890.png)





展示：

![image-20210923222235637](C:\Users\liumeng\AppData\Roaming\Typora\typora-user-images\image-20210923222235637.png)

![image-20210923222303410](C:\Users\liumeng\AppData\Roaming\Typora\typora-user-images\image-20210923222303410.png)

注意箭头的请求****







# 4.SpringSecurity （安全需求）



> springsecurity 

> shrio



主要实现的是两种功能，认证和授权

- 功能权限
- 访问权限
- 菜单权限
- .......拦截器、过滤器、大量原生代码~ 冗余代码









# 5.错误的类型

- **HTTP 错误 404** 
  404 找不到 
  Web 服务器找不到您所请求的文件或脚本。请检查URL 以确保路径正确。 
  如果问题依然存在，请与服务器的管理员联系。 
- **HTTP 错误 405** 
  405 不允许此方法 
  对于请求所标识的资源，不允许使用请求行中所指定的方法。请确保为所请求的资源设置了正确的 MIME 类型。 
  如果问题依然存在，请与服务器的管理员联系。 
- **HTTP 错误 406** 
  406 不可接受 
  根据此请求中所发送的“接受”标题，此请求所标识的资源只能生成内容特征为“不可接受”的响应实体。 
  如果问题依然存在，请与服务器的管理员联系。 
- **HTTP 错误 407** 
  407 需要代理身份验证 
  在可为此请求提供服务之前，您必须验证此代理服务器。请登录到代理服务器，然后重试。 
  如果问题依然存在，请与 Web 服务器的管理员联系。 
- **HTTP 错误 412** 
  412 前提条件失败 
  在服务器上测试前提条件时，部分请求标题字段中所给定的前提条件估计为FALSE。客户机将前提条件放置在当前资源 metainformation（标题字段数据）中，以防止所请求的方法被误用到其他资源。 
  如果问题依然存在，请与 Web 服务器的管理员联系。 
- **HTTP 错误 414** 
  414 Request-URI 太长 
  Request-URL太长，服务器拒绝服务此请求。仅在下列条件下才有可能发生此条件： 
  客户机错误地将 POST 请求转换为具有较长的查询信息的 GET 请求。 
  客户机遇到了重定向问题（例如，指向自身的后缀的重定向前缀）。 
  服务器正遭受试图利用某些服务器（将固定长度的缓冲区用于读取或执行 Request-URI）中的安全性[漏洞](http://www.2cto.com/kf/201107/96225.html)的客户干扰。 
  如果问题依然存在，请与 Web 服务器的管理员联系。 
- **HTTP 错误 500** 
  500 服务器的内部错误 
  Web 服务器不能执行此请求。请稍后重试此请求。 
  如果问题依然存在，请与 Web服务器的管理员联系。 
- **HTTP 错误 501** 
  501 未实现 
  Web 服务器不支持实现此请求所需的功能。请检查URL 中的错误，如果问题依然存在，请与 Web服务器的管理员联系。 
- **HTTP 错误 502** 
  502 网关出错 
  当用作网关或代理时，服务器将从试图实现此请求时所访问的upstream 服务器中接收无效的响应。 
  如果问题依然存在，请与 Web服务器的管理员联系。



# 6.代码托管

![image-20211026221406694](C:\Users\liumeng\AppData\Roaming\Typora\typora-user-images\image-20211026221406694.png)



![image-20211026221513399](C:\Users\liumeng\AppData\Roaming\Typora\typora-user-images\image-20211026221513399.png)



![image-20211026221842549](C:\Users\liumeng\AppData\Roaming\Typora\typora-user-images\image-20211026221842549.png)



# N.项目实操



## 1.前后端分离（程序运行模块）

# 
