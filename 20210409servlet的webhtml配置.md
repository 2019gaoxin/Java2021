### 复习

Tips1.与代码相关的软件和插件安装路径必须是英文

​	2.和代码相关一切不要用中文目录

​	3.代码放在一个目录

​	4.工具放在一个目录

### 1.Tomcat安装、配制步骤

•一、安装/解压Java运行环境JDK

•二、正确配置JDK环境变量JAVA_HOME

•三、安装/解压Tomcat应用服务器

•四、启动Tomcat服务器startup.bat

​	linux启动startup.sh;以后调试服务器一般就在linux上进行。

​	以我自己的为例D:\AppDownload\apache-tomcat-9.0.29\bin  -----startup.bat   启动。

•五、访问Tomcat服务器 http://localhost:8080/，可以访问代表安装成功。



### 2.Tomcat目录结构

•bin：Tomcat可执行文件目录（如startup.bat、shutdown.bat）//这是windows系统的

•conf：配置文件目录（server.xml服务器配置信息，如端口、主机等）

•lib：类库目录

•webapps：Web项目（站点）目录

•logs：日志存放目录

•work：运行生成的最终文件存放目录

•temp：临时文件存放目录

要是用以前的Tomcat   目录标注好版本



现在我们的Tomcat已经安装好了，我安装到了以下路径，接下来在这个路径下创建一个demo文件夹   用于存放网页  

D:\AppDownload\apache-tomcat-9.0.29\webapps   创建demo



### 思考：如何共享网页？

互联网的最初用途浅显的说可以是共享数据，所以要用web服务器，我们把网页放在服务器上

网址 localhost:8080  用服务器访问  localhost:8080 

### 如何查看自己的IP地址让其他人来访问自己的服务器呢？

win+R 输入cmd  再输入ipconfig来查看自己的IP地址，方便其他人来访问自己的服务器。   

测试网址：10.7.90.200:8080/demo/

​	PS：这里只是再用自己电脑访问老师电脑服务器的一个测试；

这就和上面对应起来了，其他人就可以访问你demo下的静态网页

### 补充

```
静态网页：
	请求服务器上的网页时，服务器不对网页文件做任何处理，读取文件直接当做响应传给浏览器
动态网页：
	服务器在响应之前，需要依据请求的参数、标头或实际服务器上的状态，以程序的方式动态产生响应内容
动态网页技术：JSP、ASP、PHP等

```



在浏览器地址里面检查  网络   看方法是GET或者POST

### 配置tomcat的用户和密码

​	思考：为什么要配置用户和密码？

​	访问Tomcat服务器 http://localhost:8080/会自动进入tomcat的管理界面 ，其中有的功能需要用户和密码才能访问。

​	D:\AppDownload\apache-tomcat-9.0.29\conf    ---tomcat-user 用notepad++打开可以配置用户名和密码

​	不同的角色有不同的权限，这里不深入。

```html
  <role rolename="tomcat"/>
  <role rolename="role1"/>
  <user username="tomcat" password="<must-be-changed>" roles="tomcat"/>
  <user username="both" password="<must-be-changed>" roles="tomcat,role1"/>
  <user username="role1" password="<must-be-changed>" roles="role1"/>
```

### servlet简介

```
Servlet: 服务器端小程序
Servlet是运行在服务器上，在服务器端调用、执行，按Servlet规范编写的Java类
对客户端的请求进行处理
向客户端返回响应

```



思考：我们如何用java来启动Tomcat呢？

### 在eclipse中配置Tomcat

打开eclipse

​	1.windows - preference

​	2.server - Runtime environment

​	3.ADD

​	4.Apqache- Tomcat 9

​	5.选择tomcat所在目录

接下来我们在eclipse中创建一个动态网站项目

​	1.File----New

​	2.Dynamic Web project

​	3.next---记住最后要勾选 Generate web.xml deployment descriptor

​	4.finish

### 这个项目的java部分

​	1.找到项目中的Java Resources

​	2.src中创建包和类

PS：这个包之后会用到

在类中的代码

```java
package com.gx.demo;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.Servlet;
import javax.servlet.ServletConfig;
import javax.servlet.ServletException;
import javax.servlet.ServletRequest;
import javax.servlet.ServletResponse;

public class Servlet1 implements Servlet {

	@Override
	public void init(ServletConfig config) throws ServletException {
		// TODO Auto-generated method stub
		
	}

	@Override
	public ServletConfig getServletConfig() {
		// TODO Auto-generated method stub
		return null;
	}
 
	@Override
	public String getServletInfo() {
		// TODO Auto-generated method stub
		return null;
	}


	@Override
	public void service(ServletRequest req, ServletResponse res) throws ServletException, IOException {
		// TODO Auto-generated method stub
		res.setContentType("text/html;charset=utf-8");
		PrintWriter writer = res.getWriter();
		
		writer.write("hellow");
		writer.print("hello1");//多加了一个判断是否为空
		writer.println("hello2");//除了换行符多加了锁，线程安全
		
		
	}
	@Override
	public void destroy() {
		// TODO Auto-generated method stub
		
	}


}

```

我们之后要配置项目中的WebContent 下的web.xml

### 配置文件的问题

### 记住配置文件时一定是包名+类名

```html
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://xmlns.jcp.org/xml/ns/javaee" xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd" id="WebApp_ID" version="4.0">
  <display-name>ch03-servlet</display-name>
  <welcome-file-list>
    <welcome-file>index.html</welcome-file>
    <welcome-file>index.htm</welcome-file>
    <welcome-file>index.jsp</welcome-file>
    <welcome-file>default.html</welcome-file>
    <welcome-file>default.htm</welcome-file>
    <welcome-file>default.jsp</welcome-file>
  </welcome-file-list>
  <servlet>
  	<servlet-name>s1</servlet-name>
  	<servlet-class>com.gx.demo.Servlet1</servlet-class>
  </servlet>
  
  <servlet-mapping>
  	<servlet-name>s1</servlet-name>
  	<url-pattern>/fa</url-pattern>
  </servlet-mapping>
  
  
</web-app>
```

PS:

​	1.一开始运行，注意首先出现的页面是web.xml，因为是配置文件，考虑到安全问题，一般Java会做安全处理，你无法正常访问。

​	2.所以在网址后面加/fa（以上面的代码为例）去访问静态页面

​	3.这里很多人运行会报错，原因就是配置文件的问题。我一开始出错就是不知道包名后面要跟着类名。

​	