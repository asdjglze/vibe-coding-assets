---
title: "Windows CMD运行含emoji Python脚本报编码错误"
usage_scenario:
    - "在Windows上运行含Unicode符号的Python脚本时终端报UnicodeEncodeError"
    - "脚本stdout显示异常但实际文件已按预期修改"
keywords:
    - "Windows CMD"
    - "GBK编码"
    - "UnicodeEncodeError"
    - "emoji"
---

Windows CMD默认使用GBK编码，运行含Unicode emoji（如\u2705 ✅）的Python脚本会抛出UnicodeEncodeError。此时不应依赖脚本stdout判断执行结果，而应直接读取cmd_queue.json文件内容验证各命令'sent'和'success'字段状态。真实执行结果以文件状态为准，而非终端输出。（来源：Bash）
