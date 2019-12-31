title: 利用redis实现分布式请求防重复提交
author: weiyonghua
tags:
  - redis
  - 锁
categories:
  - 后端
date: 2019-12-31 16:20:00
---
1.自定义注解类Token

    @Target(ElementType.METHOD)
    @Retention(RetentionPolicy.RUNTIME)
    public @interface Token {
    
        String flag() default "";
    }

2.在需要拦截的路径上加自定义注解

    **@Token**
    @RequestMapping(value = "/pda/pick-task/list")
    public RespJson getPickTask(@RequestParam("whNo") String whNo,
                                @RequestParam(value = "sourceType", required = false) Integer sourceType,
                                @RequestParam(value = "retrieveValue", required = false) String retrieveValue,
                                @RequestParam(value = "realTimePick", required = false, defaultValue = "0") int realTimePick,
                                @RequestParam(value = "taskGroupNo", required = false) String taskGroupNo) {
    ...

3.利用切面拦截请求

    @Aspect
    @Component
    public class MethodInterceptor {
        private Logger logger = LoggerFactory.getLogger(MethodInterceptor.class);

    /**
     * 重复请求拦截
     *
     * @param joinPoint
     * @return
     * @throws Throwable
     */
    @Around("@annotation(com.haiziwang.kwms.common.annotation.Token)")
    public Object repeatRequestAround(ProceedingJoinPoint joinPoint) throws Throwable {
    
        MethodSignature methodSignature = (MethodSignature) joinPoint.getSignature();
        Method currentMethod = joinPoint.getTarget().getClass().getMethod(methodSignature.getName(), methodSignature.getParameterTypes());
    
        IKMEMCache cache = KMemServiceImpl.getCache();
        //拼接签名
        StringBuilder signBuffer = new StringBuilder(currentMethod.getAnnotation(RequestMapping.class).value()[0]);
        Object[] args = joinPoint.getArgs();
        for (Object object : args) {
            if (object != null) {
                String str = "";
                try {
                    str = JSONObject.toJSONString(object);
                } catch (Exception e) {
    
                }
                signBuffer.append("^").append(str);
            }
        }
        String tokenKey = signBuffer.toString();
    
        if (StringUtils.isNotBlank(cache.readFromHash(RedisKeyType.TOKEN.getName(), tokenKey))) {
            if (StringConstants.WCS.equals(currentMethod.getAnnotation(Token.class).flag())) {
                throw new WMS3CheckedException(WMS3ExceptionCode.WCS_REPEAT_REQUEST_EXCEPTION);
            }
            if (currentMethod.getReturnType() == RespJson.class) {
                throw new WMS3CheckedException(WMS3ExceptionCode.REPEAT_REQUEST_EXCEPTION);
            }
            if (currentMethod.getReturnType() == PageRespJson.class) {
                throw new WMS3CheckedException(WMS3ExceptionCode.REPEAT_REQUEST_PAGE_EXCEPTION);
            }
        }
        cache.write4Hash(RedisKeyType.TOKEN.getName(), tokenKey, "token");
        try {
            return joinPoint.proceed();
        } finally {
            cache.deleteHashFields(RedisKeyType.TOKEN.getName(), tokenKey);
        }
    }

 