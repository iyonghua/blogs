title: 解决java8 stream无法复用问题（为要执行的每一个终端操作创建一个新的Stream链）
author: weiyonghua
tags:
  - stream
categories:
  - 后端
date: 2019-12-31 16:16:00
---
java8 stream只能进行一次终止操作，第二次终止操作异常。下面提供一种可重复使用stream的方法（**为要执行的每一个终端操作创建一个新的Stream链**）：

    Supplier<Stream<String>> streamSupplier =
        () -> Stream.of("d2", "a2", "b1", "b3", "c")
                .filter(s -> s.startsWith("a"));
    
    streamSupplier.get().anyMatch(s -> true);   // ok
    streamSupplier.get().noneMatch(s -> true);  // ok