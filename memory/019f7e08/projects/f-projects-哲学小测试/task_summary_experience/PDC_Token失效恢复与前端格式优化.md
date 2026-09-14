---
title: "PDC Token失效恢复与前端格式优化"
usage_scenario:
    - "第三方API Token失效后的自动化恢复流程设计"
    - "前端JS中数字千分位格式化的跨平台兼容处理"
    - "Vite打包Chunk间环状引用的缓存与加载问题排查"
keywords:
    - "PDC Token"
    - "Playwright"
    - "万位分组"
    - "Chunk引用"
---

## 任务描述
处理 PDC Token 失效导致的静默跳过问题，开发并线登录器自动恢复令牌，并优化前端在库册数的千分位显示格式。

## 执行过程
```mermaid
graph TD
    A[需求:Token失效恢复+前端格式优化] --> B[诊断:定位state/pdc_token_stale.json快照]
    B --> C[编写pdc_login.py:Playwright驱动Edge弹窗]
    C --> D[调试:发现browser.contexts[0]索引越界导致进程崩溃]
    D --> E[修正:改用browser.new_context()创建上下文]
    E --> F[增强:添加心跳日志与自动重开机制]
    F --> G[前端诊断:HomeView ae函数toLocaleString西方分组]
    G --> H[修复:替换为正则\B(?=(\d{4})+(?!\d))实现中国万位分组]
    H --> I[排错:整链五chunk改名custom2解决环状引用白屏]
    I --> J[验证:Playwright无头渲染确认5,0479格式生效]
    J --> K[部署:上传至服务器并校验SHA256]
```

## 任务总结
成功实现 PDC Token 失效自动恢复流程：
1. 修复 pdc_login.py 的 Context 创建 Bug，支持弹窗扫码登录并自动抓取写入 Token。
2. 将 HomeView 中的数字格式化函数改为中国万位分组（如 5,0479），并通过整链 Chunk 改名解决缓存与引用问题。
3. 完成补丁部署，站点渲染正常。
