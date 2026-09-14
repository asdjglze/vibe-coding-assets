---
title: "Windows下Gradle构建需指定Android Studio内置JDK路径"
usage_scenario:
    - "Windows终端执行gradlew构建报JAVA_HOME未设置错误"
    - "需要快速定位并使用Android Studio自带的JDK进行构建"
keywords:
    - "Gradle"
    - "JAVA_HOME"
    - "Android Studio"
    - "Windows"
---

在 Windows 环境下使用 Gradle 构建 Android 项目时，若终端报错 `ERROR: JAVA_HOME is not set and no 'java' command could be found in your PATH`，需手动指定 JAVA_HOME。本地 Android Studio 内置 JDK 路径通常为：`C:\Program Files\Android\Android Studio\jbr`。修复命令示例：`$env:JAVA_HOME = "C:\Program Files\Android\Android Studio\jbr"; .\gradlew.bat assembleDebug`。（来源：Bash）
