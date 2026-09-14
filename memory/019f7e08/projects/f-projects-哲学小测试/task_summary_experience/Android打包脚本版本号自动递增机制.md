---
title: "Android打包脚本版本号自动递增机制"
usage_scenario:
    - "Android Gradle 项目 CI/CD 流水线配置"
    - "本地构建脚本优化，避免手动修改版本号"
    - "需要频繁打包测试且区分版本的场景"
keywords:
    - "Android"
    - "打包脚本"
    - "版本号递增"
    - "PowerShell"
    - "Gradle"
---

## 任务描述
解决 Android 应用打包时版本号固定导致的新旧包混淆问题，实现自动化版本管理。

## 执行过程
```mermaid
graph TD
    A[需求:打包脚本自动递增版本号] --> B[定位build_pack.ps1打包脚本]
    B --> C[读取app/build.gradle.kts获取当前versionCode]
    C --> D[计算新版本号: versionCode + 1]
    D --> E[生成新versionName: 主版本.次版本.新版号]
    E --> F[正则替换gradle文件中的versionCode和versionName]
    F --> G[保存文件并继续后续打包流程]
```

## 任务总结
在 PowerShell 打包脚本 `build_pack.ps1` 中增加了版本号自动递增逻辑：
1. 解析 `app/build.gradle.kts` 中的 `versionCode` 和 `versionName`。
2. `versionCode` 自动加 1。
3. `versionName` 格式调整为 `major.minor.versionCode`，确保每次打包生成的 APK 版本号唯一且递增，便于安装和版本追踪。
