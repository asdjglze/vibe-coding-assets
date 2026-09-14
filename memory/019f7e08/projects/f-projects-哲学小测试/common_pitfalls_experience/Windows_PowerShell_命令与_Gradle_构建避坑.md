---
title: "Windows PowerShell 命令与 Gradle 构建避坑"
usage_scenario:
    - "在 Windows 终端执行文件复制/查询时报 ParserError 或 ParameterBindingException"
    - "Gradle 构建报错 Address already in use 端口占用"
keywords:
    - "PowerShell"
    - "Gradle"
    - "端口冲突"
    - "dir 命令"
---

在 Windows PowerShell 环境下执行文件操作时，避免使用包含复杂格式化或特殊字符（如中文变量名）的管道命令，直接使用 `dir <路径>` 或 `Get-Item <路径>` 更稳定可靠。（来源：Bash）

Android Gradle 构建遇到 `java.net.BindException: Address already in use` 端口冲突时，若确认无残留 Java/Gradle 进程，直接添加 `--no-daemon` 参数强制单进程构建即可解决，无需手动杀进程。（来源：Bash）
