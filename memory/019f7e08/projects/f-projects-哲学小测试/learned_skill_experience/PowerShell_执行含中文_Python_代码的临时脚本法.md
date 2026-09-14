---
title: "PowerShell 执行含中文 Python 代码的临时脚本法"
usage_scenario:
    - "在 Windows PowerShell 中运行包含中文或特殊字符的 Python 单行命令时报 SyntaxError"
    - "需要执行复杂的 Python 逻辑但不想手动复制粘贴到 IDE"
keywords:
    - "PowerShell"
    - "Python 脚本"
    - "转义错误"
    - "临时文件"
---

## 输入
- 需要在 PowerShell 环境中执行包含中文或复杂转义的 Python 代码
- 目标：访问本地数据库或处理特定逻辑

## 步骤
1. 避免直接在 Bash/PowerShell 命令行中使用 `-c` 参数传递含中文/特殊字符的 Python 代码（易导致转义失败）
2. 将 Python 逻辑写入临时 `.py` 文件（确保编码为 utf-8）
3. 使用 `python <脚本路径>` 执行临时脚本
4. 任务完成后立即使用 `Remove-Item` 清理临时文件

## 输出
Python 脚本成功执行并返回预期结果，无转义错误

## 注意事项
- 此方法适用于任何需要跨 Shell 传递复杂逻辑的场景
- 临时文件路径建议放在项目根目录或临时目录，避免权限问题
