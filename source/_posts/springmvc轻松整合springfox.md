title: springmvc轻松整合springfox
author: weiyonghua
tags:
  - springmvc
  - springfox
  - swigger
categories:
  - 后端
date: 2019-12-31 16:27:00
---
**springfox介绍**

Springfox的前身是swagger-springmvc,是一个开源的API doc框架，可以将我们的Controller的方法以文档的形式展现。

1.引入springfox依赖jar包

    <!-- springfox --><dependency><groupId>io.springfox</groupId><artifactId>springfox-swagger2</artifactId><version>2.0.2</version></dependency><dependency><groupId>com.fasterxml.jackson.core</groupId><artifactId>jackson-annotations</artifactId><version>2.4.4</version></dependency><dependency><groupId>com.fasterxml.jackson.core</groupId><artifactId>jackson-core</artifactId><version>2.4.4</version></dependency><dependency><groupId>com.fasterxml.jackson.core</groupId><artifactId>jackson-databind</artifactId><version>${jackson.version}</version></dependency>

2.自定义swagger 

    SwaggerConfig.java

    /**
     * The type Swagger config.
     */import org.springframework.web.servlet.config.annotation.EnableWebMvc;
    
    import springfox.documentation.swagger2.annotations.EnableSwagger2;
    
    /**
     * The type Swagger config.
     *
     * @author weiyonghua
     */@EnableWebMvc@EnableSwagger2
    publicclassSwaggerConfig{
    
    }

3.spring容器加载 applicationContext.xml

    <!--springfox swagger--><beanclass="com.chuyan.common.swagger.SwaggerConfig"/>

4.https://github.com/swagger-api/swagger-ui.git 下载swagger-ui中的dist文件夹引入项目。

5.修改swagger-ui项目中的index.html文件url地址 

![](https://static.oschina.net/uploads/space/2016/1227/085431_quYw_2485283.png)

6.项目访问地址 http://localhost:8080/${project.name}/swagger/index.html

![](https://static.oschina.net/uploads/space/2016/1227/082959_idss_2485283.png)

7.swagger注解使用参考https://my.oschina.net/zzuqiang/blog/793606