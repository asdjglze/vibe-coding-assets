---
title: "Qoder规则文件跨目录安装流程"
usage_scenario:
    - "需要将规则文件安装到工作区外的系统目录"
    - "在Qoder IDE中配置全局生效的规则"
keywords:
    - "Qoder"
    - "Rules"
    - "Copy-Item"
    - "哈希校验"
---

## 输入
- 用户提供的规则文本内容
- 目标安装路径（如 `~/.qoder/rules/`）

## 步骤
1. **确认格式**：查阅官方文档或搜索确认 Qoder Rules 的 frontmatter 格式（通常为 `trigger: always_on` 表示始终生效）。
2. **暂存文件**：由于 Write 工具无法直接操作工作区外文件，先将完整内容写入当前项目下的临时文件（如 `_tmp_rule.md`）。
3. **复制文件**：使用 PowerShell 命令 `New-Item -ItemType Directory ... -Force` 确保目标目录存在，然后使用 `Copy-Item` 将暂存文件复制到目标路径。
4. **完整性校验**：使用 `Get-FileHash` 对比源文件和目标文件的哈希值，确保传输无损。
5. **清理与记录**：删除工作区内的暂存文件，并更新记忆以记录该配置。

## 输出
规则文件成功安装至指定路径，且内容完整无误。

## 注意事项
- **中文乱码问题**：在 PowerShell 命令行中直接传递包含中文的内容（如文件名或参数）可能导致多字节字符损坏。建议：
  - 使用 ASCII 命名的临时文件（如 `_tmp_rule.md`）。
  - 或者改用 Python 脚本处理中文内容。
- **权限与路径**：用户级规则位于 `C:\Users\win-user\.qoder\rules\`，需确保目录存在。
