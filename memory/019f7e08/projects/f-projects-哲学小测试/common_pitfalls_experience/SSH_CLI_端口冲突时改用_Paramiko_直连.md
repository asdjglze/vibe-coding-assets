---
title: "SSH CLI 端口冲突时改用 Paramiko 直连"
usage_scenario:
    - "调用 SSH CLI 接口报 ReadTimeoutError 或连接超时"
    - "本地 8765 端口被占用导致无法通过 CLI 管理远程服务器"
keywords:
    - "SSH"
    - "Paramiko"
    - "端口冲突"
    - "ReadTimeout"
    - "baidupinyin"
---

本地 SSH CLI HTTP API 服务默认监听 8765 端口，若该端口被其他进程（如百度拼音输入法 baidupinyin.exe）占用，会导致 requests 连接超时（ReadTimeoutError）。此时应跳过 CLI 代理，直接使用 paramiko 库通过 SSH 协议直连服务器执行命令或传输文件。（来源：Bash, Read）
