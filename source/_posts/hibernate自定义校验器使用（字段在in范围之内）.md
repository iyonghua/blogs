title: hibernate自定义校验器使用（字段在in范围之内）
author: weiyonghua
tags:
  - 业务校验
  - ''
categories:
  - 后端
date: 2019-12-31 16:22:00
---
1.自定义注解类DigitsMustIn

    @Constraint(validatedBy = DigitsMustInValidator.class) //具体的实现
    @Target({java.lang.annotation.ElementType.METHOD,
            java.lang.annotation.ElementType.FIELD})
    @Retention(java.lang.annotation.RetentionPolicy.RUNTIME)
    @Documented
    public @interface DigitsMustIn {
        String message() default "{}不需在[{}]中"; //提示信息,可以写死,可以填写国际化的key
    
        int[] inArr();
    
        //下面这两个属性必须添加
        Class<?>[] groups() default {};
        Class<? extends Payload>[] payload() default {};
    }

2.实现DigitsMustInValidator校验服务

    public class DigitsMustInValidator implements ConstraintValidator<DigitsMustIn, Integer> {
        Integer[] ints;
    
        @Override
        public void initialize(DigitsMustIn constraintAnnotation) {
            int[] ints = constraintAnnotation.inArr();
            this.ints = ArrayUtils.toObject(ints);
        }
    
        @Override
        public boolean isValid(Integer integer, ConstraintValidatorContext constraintValidatorContext) {
            if (ArrayUtils.contains(ints, integer)) {
                return true;
            }
            constraintValidatorContext.disableDefaultConstraintViolation();//禁用默认的message的值
            //重新添加错误提示语句
            constraintValidatorContext.buildConstraintViolationWithTemplate("["+integer+"]" + "必须在" + Arrays.toString(ints) + "之内").addConstraintViolation();
    
            return false;
        }
    }

3.在需要校验的字段上加自定义注解

    @DigitsMustIn(inArr = {7, 8, 18})
    private Integer sourceType;

4.验证

    private void validate(Object validateObj) throws WMS3CheckedException {
        for (ConstraintViolation<Object> constraintViolation : Validation.buildDefaultValidatorFactory().getValidator().validate(validateObj)) {
            throw new WMS3CheckedException(WMS3ExceptionCode.UNKNOW_EXCEPTON.getCode(),
                    constraintViolation.getPropertyPath() + constraintViolation.getMessage());
        }
    }