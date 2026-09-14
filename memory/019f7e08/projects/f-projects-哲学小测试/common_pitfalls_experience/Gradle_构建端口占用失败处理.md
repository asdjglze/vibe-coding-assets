---
title: "Gradle 构建端口占用失败处理"
usage_scenario:
    - "运行 Gradle 构建命令时报 Address already in use 错误"
    - "多项目并行构建导致 Gradle Daemon 端口冲突"
keywords:
    - "Gradle"
    - "BindException"
    - "Daemon"
    - "端口占用"
---

Android Gradle 构建时可能因 `java.net.BindException: Address already in use: bind` 失败，原因是 Gradle Daemon 占用了端口。解决方案：1. 等待脚本自动重试（通常 15 秒后）；2. 若多次失败，手动执行 `./gradlew --stop` 停止所有 Daemon 进程后重试。（来源：Bash, build_pack.ps1）
