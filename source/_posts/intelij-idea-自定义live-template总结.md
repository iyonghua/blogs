title: intelij idea 自定义live template总结
author: weiyonghua
tags:
  - idea
categories:
  - 奇淫巧技
date: 2019-12-31 16:13:00
---
![](https://oscimg.oschina.net/oscnet/3e8565df04a0dc263cdb70e136fc2099ea6.jpg)

![](https://oscimg.oschina.net/oscnet/98acf6b664c5ee659e09668977b4602a4b8.jpg)

![](https://oscimg.oschina.net/oscnet/1f68b6a6bc5e8778406c45f409d578db167.jpg)

![](https://oscimg.oschina.net/oscnet/fb89692c89b6781250d990099aa719a2956.jpg)

![](https://oscimg.oschina.net/oscnet/c34d0d06d15d55c9864fd999cfe245da532.jpg)

- 日志打印

log

    private Logger logger = LoggerFactory.getLogger($CLASS$.class);

logd

    logger.debug("$METHOD_NAME$: $content$");

loge

    logger.error("$METHOD_NAME$: $content$，{}", $exception$);

logi

    logger.info("$METHOD_NAME$: $content$");

- stream

.groupBy

    .collect(Collectors.groupingBy(e -> $END$));

.join

    .collect(Collectors.joining("$END$"));

.toList

    .collect(Collectors.toList());

.toMap

    .collect(Collectors.toMap());

.toSet

    .collect(Collectors.toSet());

- Qita

map

    Map<String, $TYPE$> $VAR$ = new HashMap<>();

opt

    Optional.ofNullable($VAR$).orElse($END$);