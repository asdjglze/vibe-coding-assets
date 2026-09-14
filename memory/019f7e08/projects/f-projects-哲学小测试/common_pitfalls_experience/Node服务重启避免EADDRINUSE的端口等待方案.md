---
title: "Node服务重启避免EADDRINUSE的端口等待方案"
usage_scenario:
    - "Node服务部署后报EADDRINUSE错误"
    - "fuser -k杀进程后新服务无法启动"
    - "需要确保端口完全释放后再启动新服务"
keywords:
    - "EADDRINUSE"
    - "fuser"
    - "ss"
    - "端口释放"
    - "Node重启"
---

Node服务重启时执行`fuser -k 13000/tcp`后立即启动新进程，会导致`EADDRINUSE`错误（端口仍被旧进程残留占用）。正确做法是：先`fuser -k 13000/tcp`，再循环检测`ss -lntp | grep -q 13000`直到端口空闲，最后启动服务。完整命令：`fuser -k 13000/tcp; while ss -lntp | grep -q 13000; do sleep 1; done; cd /www/wwwroot/philosophy-test && PORT=13000 nohup node server.js >> nohup.log 2>&1 &`（来源：Bash）
