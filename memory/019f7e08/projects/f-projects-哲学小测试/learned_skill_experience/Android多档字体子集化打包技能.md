---
title: "Android多档字体子集化打包技能"
usage_scenario:
    - "需要为Android应用生成不同大小的安装包"
    - "应用包含大量字体资源需要优化体积"
    - "自动化构建流程中包含数据库同步和字体处理"
keywords:
    - "Android打包"
    - "字体子集化"
    - "Gradle构建"
    - "多档APK"
---

## 输入
- Android项目路径
- 打包脚本（如build_pack.ps1）

## 步骤
1. 后台启动打包脚本，重定向日志到文件
2. 监控日志确认环境初始化（JAVA_HOME等）及版本号递增
3. 等待数据库同步、笔画字典生成、sortOrder校验完成
4. 监控字体子集化进度（通常耗时较长，涉及大量字体文件处理）
5. 监控Gradle构建进度（分大/中/小三档依次构建）
6. 确认BUILD SUCCESSFUL并检查产物大小

## 输出
- 多个不同大小的APK文件（如large/medium/mini）
- 语料库MD5更新，确保新版本覆盖旧数据
- 字体全量子集化完成，显著减小APK体积

## 注意事项
- 字体子集化是耗时关键路径，需预留足够时间
- 使用`env -i`或类似机制模拟最小环境时需注意PATH配置
- 建议开启Gradle Configuration Cache以加速后续构建
