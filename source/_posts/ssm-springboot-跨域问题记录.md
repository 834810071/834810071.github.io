---
title: ssm + springboot 跨域问题记录
date: 2020-12-04 15:56:39
tags:java web
---

### springboot 服务端解决跨域问题

```java
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.CorsRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

@Configuration
public class WebMvcConfig implements WebMvcConfigurer {
    @Override
    public void addCorsMappings(CorsRegistry registry) {
        registry.addMapping("/**")
                .allowedOrigins("*")
                .allowedMethods("*")
                .allowedHeaders("*")
                .allowCredentials(true);
    }
}
```

### ssm + tomcat 解决跨域问题
#### tomcat配置
1. web.xml 过滤配置
 打开tomcat安装目录的conf目录，打开web.xml文件，然后在此文件的中间位置，大概460多行附近，粘贴如下代码到此文件
```xml
<filter>
        <filter-name>CORS</filter-name>
        <filter-class>com.thetransactioncompany.cors.CORSFilter</filter-class>
        <init-param>
            <param-name>cors.allowOrigin</param-name>
            <param-value>*</param-value>
        </init-param>
        <init-param>
            <param-name>cors.supportedMethods</param-name>
            <param-value>GET,POST,HEAD,PUT,DELETE</param-value>
        </init-param>
        <init-param>
            <param-name>cors.supportedHeaders</param-name>
            <param-value>Accept,Origin,X-Requested-With,Content-Type,Last-Modified</param-value>
        </init-param>
        <init-param>
            <param-name>cors.exposedHeaders</param-name>
            <param-value>Set-Cookie</param-value>
        </init-param>
        <init-param>
            <param-name>cors.supportsCredentials</param-name>
            <param-value>true</param-value>
        </init-param>
    </filter>
    <filter-mapping>
        <filter-name>CORS</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping> 
```
2. lib放入jar包:cors-filter-1.7.jar和java-property-utils-1.9.1.jar

   

3. 打开谷歌浏览器快捷方式的属性面板，然后在【目标】这个属性之后跟下面的代码：
```java
--args --disable-web-security --user-data-dir
```

### idea tomcat 配置项目注意点

1. ![image-20201204155825612](C:\Users\jxq\AppData\Roaming\Typora\typora-user-images\image-20201204155825612.png)

   【Run/Debug Configurations】 -> Deploment -> Application context: 设置为 / (如果项目名字为空的话)