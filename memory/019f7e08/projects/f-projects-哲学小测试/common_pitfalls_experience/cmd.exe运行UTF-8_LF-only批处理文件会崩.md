---
title: "cmd.exe运行UTF-8 LF-only批处理文件会崩"
usage_scenario:
    - "在Windows终端运行bat脚本时报'不是内部或外部命令'或乱码错误"
    - "排查bat脚本执行失败但脚本逻辑本身无误的原因"
keywords:
    - "cmd.exe"
    - "UTF-8"
    - "LF换行"
    - "GBK编码"
---

Windows cmd.exe 默认按 GBK 代码页 + CRLF 换行解析批处理文件。若 build.bat 是 UTF-8 编码且仅含 LF（Unix换行，CR=0），直接运行会导致中文注释乱码、命令被拆成碎片（报'此时不应有 ----'等错误），脚本无法执行。修复方案：用 PowerShell 将文件转为 CRLF 换行并保存为 GBK 编码副本后执行。（来源：Bash）
