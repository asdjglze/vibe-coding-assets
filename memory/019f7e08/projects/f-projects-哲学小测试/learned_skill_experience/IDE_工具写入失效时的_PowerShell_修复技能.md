---
title: "IDE 工具写入失效时的 PowerShell 修复技能"
usage_scenario:
    - "IDE 编辑工具报告修改成功但文件内容未变"
    - "批量替换模板字段时发现写入异常"
    - "需要确保中文字符在脚本中正确编码"
keywords:
    - "PowerShell"
    - "文件写入"
    - "码点验证"
    - "IDE 故障"
    - "UTF-8"
---

## 输入
- IDE 编辑工具（如 SearchReplace/Write）报告修改成功但磁盘文件未更新
- 需要批量替换模板中的字段名（如 source → bookTitle）

## 步骤
1. **诊断**：比对文件 LastWriteTime 确认是否真正落盘；使用 Grep 检查残留量
2. **绕过 IDE 工具**：改用 PowerShell 直接读写文件（[System.IO.File]::ReadAllText / WriteAllText）
3. **构造中文字符**：避免 shell 传参乱码，使用 Unicode 码点拼接（如 `[char]0x6458 + [char]0x81EA` = "摘自"）
4. **逐文件修复**：读取全文 → 正则/字符串替换 → 写回磁盘 → 立即验证
5. **字节级验证**：提取关键字符的 Hex 码点（如 `6458 81EA`）确保编码正确

## 输出
- 磁盘文件已实际修改，Grep 显示目标字段残留为 0
- 中文内容码点正确，无乱码

## 注意事项
- IDE 工具可能报告成功但未落盘，需以磁盘实际状态为准
- PowerShell 脚本可直接处理 UTF-8 文件，避免中间层干扰
- 涉及中文时务必用码点构造，防止终端编码破坏
