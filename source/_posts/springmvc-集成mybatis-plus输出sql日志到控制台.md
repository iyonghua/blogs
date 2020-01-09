title: springmvc 集成mybatis-plus输出sql日志到控制台
author: weiyonghua
tags:
  - mybatis
categories:
  - 后端
date: 2020-01-08 18:11:00
---
**今天做项目集成mybatis-plus的时候发下控制台没有输出mybatis的日志，下面记录下使用log4j在控制台输出日志的方法:**
方法1：在spring配置文件中增加mybatis的configuration配置
![clipboard.png](http://ww1.sinaimg.cn/large/0082pGazgy1gapbky76bjj314806c74p.jpg)

    <bean id="sqlSessionFactory" class="com.baomidou.mybatisplus.extension.spring.MybatisSqlSessionFactoryBean">
	    <property name="dataSource" ref="dataSource1"/>
	    <property name="mapperLocations" value="classpath*:mybatis.mapper/*.xml"/>
	    <property name="configuration" ref="mybatisConfiguration"></property>
    </bean>
    
    <bean id="mybatisConfiguration" class="com.baomidou.mybatisplus.core.MybatisConfiguration">
    	<property name="logImpl" value="org.apache.ibatis.logging.slf4j.Slf4jImpl"></property>
    </bean>
方法2：（推荐使用）在log4j中配置日志输出打印

![clipboard.png](http://ww1.sinaimg.cn/large/0082pGazgy1gapbqn9nvfj30v601bq2t.jpg)


log4j.properties

    log4j.logger.com.xxx.xxx.mapper=TRACE

方法3：在mybatis-config.xml中配置日志实现

    <?xml version="1.0" encoding="UTF-8"?>
    <!DOCTYPE configuration 
	PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
    "http://mybatis.org/dtd/mybatis-3-config.dtd">
    <configuration>
	    <settings>
	    	<setting name="logImpl" value="STDOUT_LOGGING"/>
	    </settings>
    </configuration>