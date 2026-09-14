---
title: "Windows 终端 Python 打印中文乱码修复"
usage_scenario:
    - "Python 脚本在 Windows 终端运行时报 UnicodeEncodeError"
    - "打印包含中文或特殊字符的数据库记录时终端报错"
keywords:
    - "Python"
    - "UnicodeEncodeError"
    - "UTF-8"
    - "Windows 终端"
---

在 Windows 命令行运行 Python 脚本时，若涉及非 ASCII 字符（如中文、特殊符号）打印到控制台，可能因默认编码（GBK）导致 `UnicodeEncodeError: 'gbk' codec can't encode character`。修复方法：在脚本开头添加 `import sys; sys.stdout.reconfigure(encoding="utf-8")` 强制标准输出为 UTF-8。（来源：Bash）
