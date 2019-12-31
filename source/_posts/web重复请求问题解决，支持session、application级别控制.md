title: web重复请求问题解决，支持session、application级别控制
author: weiyonghua
tags:
  - web请求
categories:
  - 后端
date: 2019-12-31 16:26:00
---
  最近工作中碰到一个问题，接口响应时间较长时（比如大批量查询和报表导出时）用户重复请求会造成系统越来越卡。

解决方案：

    使用自定义拦截器完美控制，支持控制级别session和application。下面附上代码备忘：

1.新增自定义注释token

    import java.lang.annotation.ElementType;
    import java.lang.annotation.Retention;
    import java.lang.annotation.RetentionPolicy;
    import java.lang.annotation.Target;

    @Target(ElementType.METHOD)
    @Retention(RetentionPolicy.RUNTIME)
    public @interface Token {}

 

2.自定义拦截器功能

    import com.alibaba.fastjson.JSONObject;
    import com.haiziwang.kwms.common.annotation.Token;
    import com.haiziwang.kwms.common.domain.util.RespJson;
    import com.haiziwang.platform.esf.common.util.JSONUtil;
    import org.apache.commons.io.IOUtils;
    import org.apache.commons.lang3.StringUtils;
    import org.slf4j.Logger;
    import org.slf4j.LoggerFactory;
    import org.springframework.web.method.HandlerMethod;
    import org.springframework.web.servlet.handler.HandlerInterceptorAdapter;
    
    import javax.servlet.http.HttpServletRequest;
    import javax.servlet.http.HttpServletResponse;
    import javax.servlet.http.HttpSession;
    import java.io.IOException;
    import java.io.PrintWriter;
    import java.lang.reflect.Method;
    import java.util.UUID;

    public class TokenInterceptor extends HandlerInterceptorAdapter {
        private static final Logger logger = LoggerFactory.getLogger(Token.class);
    
        @Override
        public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
            logger.info("TokenInterceptor preHandle execute ...");
            if (handler instanceof HandlerMethod) {
                HandlerMethod handlerMethod = (HandlerMethod) handler;
                Method method = handlerMethod.getMethod();
                Token annotation = method.getAnnotation(Token.class);
                if (annotation != null) {
                    String tokenKey = request.getServletPath() + "^" + JSONUtil.toJson(request.getParameterMap());
                    HttpSession session = request.getSession();
                    String tokenValue = (String) session.getServletContext().getAttribute(tokenKey);
                    String tx = UUID.randomUUID().toString();
                    if (StringUtils.isBlank(tokenValue)){
                        //第一次请求把请求事务id放入session
                        //session.setAttribute(tokenKey, tx);
                        //第一次请求把请求事务id放入applicationContext
                        session.getServletContext().setAttribute(tokenKey, tx);
                    } else {
                        //重复请求
                        response.setContentType("application/json;charset=UTF-8");
                        PrintWriter out = null;
                        try {
                            out = response.getWriter();
                            out.write(JSONObject.toJSONString(RespJson.buildFailureRepeatResponse()));
                            //处理结束
                            return false;
                        } catch (IOException e) {
                            e.printStackTrace();
                        } finally {
                            IOUtils.closeQuietly(out);
                        }
                    }
    
                    request.setAttribute(tokenKey, tx);
                }
                return true;
            } else {
                return super.preHandle(request, response, handler);
            }
        }
    
        @Override
        public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
            logger.info("TokenInterceptor afterCompletion execute ...");
            if (handler instanceof HandlerMethod) {
                HandlerMethod handlerMethod = (HandlerMethod) handler;
                Method method = handlerMethod.getMethod();
                Token annotation = method.getAnnotation(Token.class);
                if (annotation != null) {
                    String tokenKey = request.getServletPath() + "^" + JSONUtil.toJson(request.getParameterMap());
                    HttpSession session = request.getSession();
                    //session级别控制从session中取值，这里控制级别是application级别
                    //String sessionTokenValue = (String) session.getAttribute(tokenKey);
                    String applicationTokenValue = (String) session.getServletContext().getAttribute(tokenKey);
    
                    String requestTokenValue = (String) request.getAttribute(tokenKey);
                    //判断是不是第一次请求
                    if (StringUtils.isNotBlank(applicationTokenValue) && StringUtils.isNotBlank(requestTokenValue) && applicationTokenValue.equals(requestTokenValue)){
                        //释放锁
                        session.getServletContext().removeAttribute(tokenKey);
                    }
                }
            }
    
            super.afterCompletion(request, response, handler, ex);
        }
    
    }

3.在springmvc xml配置文件中配置自定义拦截器

    <!-- 拦截器配置 -->
    <mvc:interceptors>
        <!-- 配置Token拦截器，防止用户重复提交数据 -->
        <mvc:interceptor>
            <!--这个地方时你要拦截得路径 我这个意思是拦截所有的URL-->
            <mvc:mapping path="/**"/>
            <!--class文件路径改成你自己写的拦截器路径！！ -->
            <bean class="com.xxx.TokenInterceptor"/>
        </mvc:interceptor>
    </mvc:interceptors>

4.在需要拦截重复接口的地方加注解

    @RequestMapping("/mst-wh-config/list")
    @Token
    public RespJson getMstWhConfigList(MstWhConfigReqDto reqDto) {}

小伙伴们愉快的玩耍吧~~~