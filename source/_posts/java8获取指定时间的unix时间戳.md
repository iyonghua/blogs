title: java8获取指定时间的unix时间戳
author: weiyonghua
tags:
  - java8
categories:
  - 后端
date: 2019-12-31 16:17:00
---
    //获取每天11点的时间戳，其他需求自己扩展
    Function<Integer, Long> toEpochMilli = hour ->
            LocalDateTime.of(
                    LocalDate.now().getYear(), LocalDate.now().getMonth(), LocalDate.now().getDayOfMonth(),
                    11, 0, 0, 0
            )
                    .toInstant(ZoneOffset.ofHours(hour))
                    .toEpochMilli();

    //获取当前时间时间戳
    System.currentTimeMillis();
    Clock.systemUTC().millis();
    Instant.now().toEpochMilli();
    LocalDateTime.now().toInstant(ZoneOffset.ofHours(8)).toEpochMilli();

 