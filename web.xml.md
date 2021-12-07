```java
基于ContextLoaderListener实现
这种方式只适用于Servlet2.4及以上规范的Servlet，需要在web.xml中添加:
<!-- 指定Spring配置文件的位置。多个配置文件以逗号分隔 -->
<context-param>
    <param-name>contextConfigLocation</param-value>
</context-param>
<!-- 指定以ContextLoaderListener方式启动Spring容器 -->
<Listener>
    <Listener-class>
        org.springframework.web.context.ContextLoaderListener
    </listener-class>
</listener>
        
```

```java
基于ContextLoaderServlet实现
该方式需要在web.xml中添加如下代码：
<!-- 指定Spring配置文件的位置，多个配置文件以逗号分隔 -->
<context-param>
    <param-name>contextConfigLocation</param-name>
    <param-value>classpath:applicationContext.xml</param-value>
</context-param>
<!-- 指定以Servlet方式启动Spring容器 -->
<servlet>
        <servlet-name>context</servlet-name>
        <servlet-class>
            org.springframework.web.context.ContextLoaderServlet
        </servlet-class>
        <load-on-startup>1</load-on-startup>
</servlet>
```

