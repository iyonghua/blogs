title: 自定义request链路跟踪
author: weiyonghua
tags:
  - 链路追踪
categories:
  - 后端
date: 2019-12-31 16:21:00
---
1.自定义注解类

    @Target(ElementType.METHOD)
    @Retention(RetentionPolicy.RUNTIME)
    public @interface Trace {
    
        String businessName() default "";
    
    }

2.在需要拦截的方法上加自定义注解[@Trace](https://my.oschina.net/u/876728)

    @Trace(businessName = "线上复核扫描出库单号")
    @Token
    @RequestMapping(value = "/mst-obd-review/checkorderno", method = {RequestMethod.POST, RequestMethod.GET})
    public RespJson checkorderno(@RequestParam("orderNo") String orderNo,HttpServletRequest request) {
    ...

3.自定义拦截器

    public class RpcTraceInterceptor extends HandlerInterceptorAdapter {
        @Override
        public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
            **TraceContext traceContext = (TraceContext) TraceContext.ctx.get();**
            //清除traceid
            **traceContext.clear();**
            //重新生成traceid
            **MDC.put("traceId", traceContext.getTraceId());**
            return super.preHandle(request, response, handler);
        }
    
        @Override
        public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
            //请求结束情况traceid
            **MDC.remove("traceId");**
    
            super.afterCompletion(request, response, handler, ex);
        }
    }

    public class TraceContext {
        public static ThreadLocal ctx = new InheritableThreadLocal() { //此处用InheritableThreadLocal保证可以在子线程得到相同的traceId
            @Override
            protected TraceContext initialValue() {
                return new TraceContext();
            }
        };
    
        private String traceId;
    
        public String getTraceId() {
            if (traceId == null || "".equals(traceId)) {
                int hashCodeV = UUID.randomUUID().toString().hashCode();
                if (hashCodeV < 0) {//有可能是负数
                    hashCodeV = -hashCodeV;
                }
                // 0 代表前面补充0
                // 4 代表长度为4
                // d 代表参数为正数型
                traceId = String.format("%012d", hashCodeV);
            }
            return traceId;
        }
    
        public void clear() {
            traceId = null;
        }
    }