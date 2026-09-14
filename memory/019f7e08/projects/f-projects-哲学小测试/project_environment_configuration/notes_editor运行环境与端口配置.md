---
title: "notes_editor运行环境与端口配置"
usage_scenario:
    - "启动或重启notes_editor服务时"
    - "排查服务无法启动的端口冲突问题时"
keywords:
    - "notes_editor"
    - "Python"
    - "8973端口"
    - "端口占用"
---

项目notes_editor使用Python运行，默认监听8973端口。启动时需确保端口未被占用（如遇到WinError 10013），可通过`netstat -ano | findstr 8973`检查并杀旧进程后重启
