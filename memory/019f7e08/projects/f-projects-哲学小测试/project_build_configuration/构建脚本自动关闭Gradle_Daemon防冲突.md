---
title: "构建脚本自动关闭Gradle Daemon防冲突"
usage_scenario:
    - "编写或修改 Android 项目的自动化构建/打包脚本时"
    - "配置 CI/CD 流水线中涉及多次 Gradle 调用的步骤时"
keywords:
    - "Gradle"
    - "Daemon"
    - "端口冲突"
    - "构建脚本"
---

Android Gradle 构建脚本（如 build.bat、build_pack.ps1）必须在每次执行 `assemble` 或 `build` 任务前，先执行 `gradlew --stop` 命令强制停止所有 Gradle 守护进程，并等待数秒以确保端口释放，从而彻底避免多档位连续打包时的端口冲突问题。
