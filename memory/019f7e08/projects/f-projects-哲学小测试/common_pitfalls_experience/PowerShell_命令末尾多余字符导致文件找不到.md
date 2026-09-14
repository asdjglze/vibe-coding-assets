---
title: "PowerShell 命令末尾多余字符导致文件找不到"
usage_scenario:
    - "终端运行脚本时报 No such file or directory"
    - "不确定是路径问题还是文件名拼写错误"
keywords:
    - "PowerShell"
    - "文件路径"
    - "命令输入"
    - "Errno 2"
---

在 PowerShell 或终端中运行 Python 脚本时，如果命令末尾误输入了特殊字符（如 `+`），会导致 Bash 工具报 `[Errno 2] No such file or directory` 错误。例如 `python fix_noyear.py+` 会因文件名不匹配而失败。解决方案：仔细检查命令行参数，确保文件名和路径完全正确，无多余符号。（来源：Bash）
