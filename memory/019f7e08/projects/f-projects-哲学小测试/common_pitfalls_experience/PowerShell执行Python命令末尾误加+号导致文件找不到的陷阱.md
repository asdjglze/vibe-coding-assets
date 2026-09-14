---
title: "PowerShell执行Python命令末尾误加+号导致文件找不到的陷阱"
usage_scenario:
    - "Windows环境下运行Python脚本时报No such file or directory"
    - "PowerShell命令行执行脚本后出现奇怪的文件名错误"
keywords:
    - "PowerShell"
    - "Python"
    - "文件路径"
    - "命令格式"
---

在 Windows PowerShell 中执行 Python 脚本时，若命令末尾误输入了 `+` 号（如 `python script.py+`），会导致系统尝试查找名为 `script.py+` 的文件从而报错 `No such file or directory`。修复方法：确保命令末尾无多余字符，直接运行 `python fix_noyear.py`。（来源：Bash）
