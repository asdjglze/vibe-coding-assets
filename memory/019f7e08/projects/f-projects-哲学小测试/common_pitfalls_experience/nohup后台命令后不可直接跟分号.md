---
title: "nohup后台命令后不可直接跟分号"
usage_scenario:
    - "执行含nohup的复合命令时报syntax error near unexpected token ';'"
    - "后台启动Node服务后需等待再验证端口但命令卡住"
    - "Bash脚本中组合nohup与sleep等命令失败"
keywords:
    - "nohup"
    - "Bash语法"
    - "后台进程"
    - "分号错误"
---

在 Bash 中使用 `nohup command &` 启动后台进程时，若后续紧跟 `;`（如 `nohup node server.js > nohup.out 2>&1 &; sleep 3`），会导致语法错误：`bash: -c: line 1: syntax error near unexpected token ';'`。正确写法是：① 使用 `&&` 连接后续命令（`nohup node server.js > nohup.out 2>&1 & && sleep 3`），或 ② 拆分为独立命令分步执行（先启动，再 sleep + 验证）。（来源：Bash）
