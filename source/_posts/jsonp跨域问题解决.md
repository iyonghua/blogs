title: jsonp跨域问题解决
author: weiyonghua
tags:
  - 跨域
categories:
  - 后端
date: 2019-12-31 16:29:00
---
ajax接口调用过程中经常会遇到json跨域问题，解决方法如下：

[****jsonp方式暂不使用****]

**1.使用getScript方式解决**

$.getScript('http://int.dpool.sina.com.cn/iplookup/iplookup.php?format=js',function(){

console.log(remote_ip_info.city);

});

 

**2.从服务器端解决（新增自定义filter）**

<1> *CORSFilter.java*

    publicclassCORSFilterimplementsFilter {
        @Override
        public void init(FilterConfig filterConfig) throws ServletException {
    
        }
    
        @Override
        public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws IOException, ServletException {
            /*
            Access-Control-Allow-Headers: Origin, X-Atmosphere-tracking-id, X-Atmosphere-Framework, X-Cache-Date, Content-Type, X-Atmosphere-Transport, *
            Access-Control-Allow-Methods: POST, GET, OPTIONS , PUT
            Access-Control-Allow-Origin: *
            Access-Control-Request-Headers: Origin, X-Atmosphere-tracking-id, X-Atmosphere-Framework, X-Cache-Date, Content-Type, X-Atmosphere-Transport,*
            */
            ((HttpServletResponse)response).setHeader("Access-Control-Allow-Headers", "Origin, X-Atmosphere-tracking-id, X-Atmosphere-Framework, X-Cache-Date, Content-Type, X-Atmosphere-Transport, *");
    //        ((HttpServletResponse)response).setHeader("Access-Control-Allow-Methods", "POST, GET, OPTIONS , PUT");
            ((HttpServletResponse)response).setHeader("Access-Control-Allow-Origin", "*");
    //        ((HttpServletResponse)response).setHeader("Access-Control-Request-Headers", "Origin, X-Atmosphere-tracking-id, X-Atmosphere-Framework, X-Cache-Date, Content-Type, X-Atmosphere-Transport,*");//        ((HttpServletResponse)response).setContentType("application/json");//        ((HttpServletResponse)response).setHeader("Connection", "keep-alive");chain.doFilter(request, response);
        }
    
        @Overridepublicvoiddestroy() {
    
        }
    }

<2>*web.xml*新增过滤器配置

 

    <filter>
        <filter-name>CORSFilter</filter-name>
        <filter-class>com.chuyan.common.filter.CORSFilter</filter-class>
      </filter>
      <filter-mapping>
        <filter-name>CORSFilter</filter-name>
        <url-pattern>/*</url-pattern>
      </filter-mapping>

<3>spring容器加载过滤器 *applicationContext.xml  （可选，需要注入属性的时候配置。这里可以不用配置）*

    <bean id="CORSFilter"class="com.chuyan.common.filter.CORSFilter"/>

 

 