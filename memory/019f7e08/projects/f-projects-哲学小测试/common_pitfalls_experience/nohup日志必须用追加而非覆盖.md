---
title: "nohup日志必须用>>追加而非>覆盖"
usage_scenario:
    - "nohup日志被覆盖导致无法排查历史错误"
    - "SSE流中断后找不到原始报错堆栈"
    - "部署脚本中nohup命令日志丢失问题"
keywords:
    - "nohup"
    - "日志覆盖"
    - ">>追加"
    - "Bash"
---

使用 nohup 启动服务时，`>` 会覆盖已有日志文件，导致历史日志被销毁，无法追溯已发生的错误；必须改用 `>>` 追加模式保留完整日志链。正确命令示例：`PORT=13000 nohup node server.js >> nohup.log 2>&1 &`（来源：Bash）
