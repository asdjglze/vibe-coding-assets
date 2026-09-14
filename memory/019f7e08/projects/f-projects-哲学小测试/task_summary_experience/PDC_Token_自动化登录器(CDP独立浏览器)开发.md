---
title: "PDC Token 自动化登录器(CDP独立浏览器)开发"
usage_scenario:
    - "需要绕过自动化检测获取第三方网站 Cookie/Token"
    - "Playwright/Selenium 托管浏览器因反爬或跳转导致进程崩溃的场景"
    - "需要持久化浏览器会话数据以便后续恢复"
keywords:
    - "PDC登录器"
    - "CDP协议"
    - "Playwright崩溃"
    - "Token持久化"
---

## 任务描述
为 PDC 平台开发自动化登录器，解决 Token 失效后人工干预繁琐的问题，实现弹窗登录、自动抓取 Token 并写入共享状态。

## 执行过程
```mermaid
graph TD
    A[需求: PDC Token 失效恢复] --> B[初版: Playwright 托管 Edge]
    B --> C[问题: 页面跳转触发 TargetClosedError 导致驱动崩溃]
    C --> D[二版: 增加抗崩重试与心跳日志]
    D --> E[问题: 临时会话目录随进程销毁，Token 无法持久化]
    E --> F[三版: 采用 CDP 协议启动独立浏览器进程]
    F --> G[优势: 浏览器独立存活，会话落盘至 state/pdc_browser/]
    G --> H[结果: 成功捕获 localStorage user-token 并验证]
```

## 任务总结
开发了 `pdc_login.py` v3 版本。核心改进是放弃 Playwright 托管模式，改用 `subprocess` 启动带 `--remote-debugging-port=9223` 的独立 Edge 进程。通过 WebSocket (websocket-client) 连接 CDP 端口轮询 `localStorage.getItem('user-token')`。即使窗口关闭，也能从磁盘持久化的 DevTools Profile 中打捞 Token。
