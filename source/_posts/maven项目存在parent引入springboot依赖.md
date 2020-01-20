title: maven项目存在parent引入springboot依赖
author: weiyonghua
tags:
  - maven
categories:
  - 后端
date: 2020-01-20 11:09:00
---



##对于已经存在parent依赖的mavne项目怎么使用springboot项目依赖呢？

*step 0： 我们可以在依赖管理中添加spring-boot-dependencies的依赖*

     <dependency>
	    <groupId>org.springframework.boot</groupId>
	    <artifactId>spring-boot-dependencies</artifactId>
	    <version>2.1.1.RELEASE</version>
	    <type>pom</type>
	    <scope>import</scope>
    </dependency>


*step 1：在pom.xml文件的插件管理中添加springboot的maven插件，并指定在重新打包的时候引入springboot*

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
                <version>2.1.1.RELEASE</version>
                <executions>
                    <execution>
                        <goals>
                            <goal>repackage</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>
        </plugins>
    </build>