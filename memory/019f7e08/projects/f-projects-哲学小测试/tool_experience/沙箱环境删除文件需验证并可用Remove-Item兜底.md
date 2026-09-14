---
title: "沙箱环境删除文件需验证并可用Remove-Item兜底"
usage_scenario:
    - "删除项目文件后验证结果"
    - "处理构建产物残留问题"
    - "排查删除未生效的文件操作"
keywords:
    - "DeleteFile失效"
    - "Remove-Item验证"
    - "删除验证"
    - "AGP增量残留"
---

## 使用场景
在沙箱环境中删除项目文件后需确认文件真正消失。

## 使用方法
删除文件后必须立即验证：用 Bash 执行 Test-Path 或 Get-ChildItem 确认文件已不存在。

## 注意事项
DeleteFile 工具在沙箱环境可能返回删除成功但文件实际未落盘删除（路径相同、时间戳未变、内容仍在），后续构建仍会打包进产物；改用 Bash 的 Remove-Item 删除可真正生效。删除后立即 Test-Path 验证，若仍存在则用 Remove-Item 重删。构建类残留（如 AGP 增量打包 zip 空洞）需同时删除旧 APK 与 intermediates 中间产物再构建。
