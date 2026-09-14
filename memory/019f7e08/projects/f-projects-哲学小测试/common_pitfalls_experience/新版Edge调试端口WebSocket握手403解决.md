---
title: "新版Edge调试端口WebSocket握手403解决"
usage_scenario:
    - "使用Python websocket-client连接浏览器DevTools调试端口时报403 Forbidden"
    - "自动化控制浏览器时遇到WebSocket连接被拒绝"
keywords:
    - "Edge"
    - "DevTools"
    - "WebSocket"
    - "403 Forbidden"
    - "suppress_origin"
---

新版 Edge/Chrome (111+) 的调试端口 (`--remote-debugging-port`) 默认拒绝带 Origin 头的 WebSocket 握手，导致 `websocket-client` 连接报 `Handshake status 403 Forbidden`。解决方法：1. 启动浏览器时添加 `--remote-allow-origins=*` 参数；2. 客户端连接时使用 `suppress_origin=True` 参数不发送 Origin 头。（来源：Bash, websocket-client）
