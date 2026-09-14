---
title: "Qoder文件编辑工具无法操作工作区外文件"
usage_scenario:
    - "使用Qoder内置文件工具修改IDE配置或插件目录时报错"
    - "需要清理或删除工作区之外的配置文件"
keywords:
    - "Qoder"
    - "沙箱限制"
    - "DeleteFile"
    - "工作区外"
---

Qoder IDE的文件编辑工具（如DeleteFile、SearchReplace等）受沙箱限制，无法操作工作区外的文件（如 `C:\Users\win-user\.qoder\` 下的插件配置）。若遇到 `error: code = 45405 message = can not edit the file outside the projects` 错误，必须改用命令行（Bash）执行删除或修改操作。（来源：DeleteFile, Bash）
