---
title: "Gradle 编译与 JDK 环境配置"
usage_scenario:
    - "执行项目编译任务时确定正确的命令和参数"
    - "排查编译失败或环境变量未生效问题时定位 JDK 路径"
keywords:
    - "Gradle"
    - "编译命令"
    - "JDK 路径"
    - "Android Studio"
---

项目使用 Gradle 构建，编译命令为 `./gradlew.bat :app:compileDebugKotlin`，运行环境需设置 JAVA_HOME 指向 Android Studio 的 jbr 路径 (如 `C:\Program Files\Android\Android Studio\jbr`)。
