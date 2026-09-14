---
title: "Windows下SSH连接优先使用paramiko"
usage_scenario:
    - "在Windows终端执行SSH/SFTP命令失败时"
    - "编写跨平台自动化运维脚本时选择连接库"
keywords:
    - "Windows"
    - "OpenSSH兼容性"
    - "paramiko"
    - "SSH连接"
---

在Windows环境下，系统自带的OpenSSH客户端可能无法正确加载某些ed25519格式的私钥（报invalid format），但Python的paramiko库可以正常加载。因此，在Windows上执行SSH连接或SFTP传输时，应优先使用基于paramiko的脚本方案，避免直接使用scp等依赖系统OpenSSH的工具。
