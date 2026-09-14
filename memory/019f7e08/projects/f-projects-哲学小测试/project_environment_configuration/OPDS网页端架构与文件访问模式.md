---
title: "OPDS网页端架构与文件访问模式"
usage_scenario:
    - "排查在线预览被IDM拦截问题时定位服务器响应头配置"
    - "分析COPS书城文件分发机制与权限控制逻辑"
keywords:
    - "OPDS"
    - "nginx门禁"
    - "COPS"
    - "inline"
    - "fetch"
---

项目网页端架构：腾讯云VPS nginx:8890作为登录门禁层，反代至群晖COPS服务(127.0.0.1:8891)。COPS原生提供两种文件访问模式：/inline/用于在线阅读 (响应头 Content-Disposition: inline)，/fetch/用于直接下载 (响应头 Content-Disposition: attachment)。
