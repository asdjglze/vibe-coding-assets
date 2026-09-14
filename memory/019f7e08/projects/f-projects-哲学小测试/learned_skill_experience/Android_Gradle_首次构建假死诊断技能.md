---
title: "Android Gradle 首次构建假死诊断技能"
usage_scenario:
    - "Android 项目构建时终端长时间无输出或进度条不动"
    - "怀疑 Gradle 构建卡死但无法确定是网络问题还是正常下载"
    - "升级 AGP 或 Gradle 版本后首次构建耗时过长"
keywords:
    - "Gradle"
    - "AGP"
    - "构建假死"
    - "依赖下载"
    - "Android"
---

## 输入
- Android Gradle 构建过程长时间无输出或进度条停滞

## 步骤
1. 检查 Gradle 缓存目录（如 `~/.gradle/caches/modules-2`）是否有最近几分钟内修改的文件（特别是 `.pom` 或 `.module` 文件），确认正在下载依赖。
2. 检查 Java 进程是否活跃（查看 CPU 占用和内存增长），确认构建线程未挂起。
3. 若上述两项均正常，则判定为首次构建静默下载 AGP 及插件依赖（Plain 模式不显示进度），需耐心等待而非中断。

## 输出
明确判断当前状态是“正常下载中”还是“真正卡死”，避免误操作中断构建。

## 注意事项
- 首次构建或升级 AGP/Gradle 版本后，静默下载可能持续 5-15 分钟。
- Windows 环境下可使用 PowerShell 命令监控缓存文件最后写入时间和 Java 进程资源占用。
