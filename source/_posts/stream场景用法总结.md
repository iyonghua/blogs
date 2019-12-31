title: stream场景用法总结
author: weiyonghua
tags:
  - stream
categories:
  - 后端
date: 2019-12-31 16:14:00
---
    Map taskNoTaskMap = taskList.stream().collect(HashMap::new, (m,v)->m.put(v.getTaskNo(), v),HashMap::putAll);

    Map<String, Integer> taskNoPickedMap = order.getSkuList()
                        .stream()
                        .collect(
                                Collectors.groupingBy(WcsPickTaskReqDto.Body.Order.Sku::getRemark,
                                Collectors.summingInt(WcsPickTaskReqDto.Body.Order.Sku::getPickupAmount))
                        );

    Set<Long> allLackObdOrderSysNoSet = obdOrderLineList
                    .stream()
                    .collect(
                            Collectors.groupingBy(
                                    ObdOrderLine::getOrderSysNo,
                                    Collectors.collectingAndThen(
                                            Collectors.toSet(),
                                            list -> list.stream().allMatch(ol -> ol.getQtyPicked().equals(0))
                                    )
                            )
                    )
                    .entrySet()
                    .stream()
                    .filter(m -> m.getValue())
                    .collect(Collectors.toMap(m -> m.getKey(), m -> m.getValue()))
                    .keySet();

 