---
title: "高德天气API平台不匹配错误修复"
usage_scenario:
    - "调用高德天气或IP定位接口时报USERKEY_PLAT_NOMATCH错误"
    - "高德Key配置后无法调用Web服务接口"
keywords:
    - "高德API"
    - "平台不匹配"
    - "Web服务"
    - "Key类型"
---

高德地图API报错 `USERKEY_PLAT_NOMATCH` 表示 Key 类型与接口不匹配。高德 Key 分 Web 服务、JS API、SDK 等类型，调用 `restapi.amap.com/v3/ip` 或 `weatherInfo` 等后端接口必须使用 **Web 服务** 类型的 Key。若报错此错误，需登录高德开放平台控制台，新建一个类型为“Web 服务”的 Key，替换配置文件中的旧 Key。（来源：Grep/对话分析）
