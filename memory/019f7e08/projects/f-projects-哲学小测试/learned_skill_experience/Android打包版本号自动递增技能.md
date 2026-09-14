---
title: "Android打包版本号自动递增技能"
usage_scenario:
    - "每次打包APK前需要自动递增版本号"
    - "防止因版本号相同导致安装覆盖问题"
keywords:
    - "Android"
    - "版本号"
    - "build.gradle"
    - "打包脚本"
---

## 输入
- Android 项目的 `app/build.gradle.kts` 文件

## 步骤
1. 读取 `build.gradle.kts` 内容
2. 使用正则匹配 `versionCode = <数字>` 和 `versionName = "<字符串>"`
3. 将 `versionCode` 解析为整数并 +1
4. 将 `versionName` 更新为 `<原主版本号>.<新versionCode>`（如 1.0 → 1.3）
5. 将修改后的文本写回文件

## 输出
`build.gradle.kts` 中的版本号已自动递增

## 注意事项
- 建议在打包脚本（如 `build_pack.ps1`）中集成此逻辑，确保每次打包前自动递增
- 避免硬编码版本号，防止重复安装或混淆新旧包
