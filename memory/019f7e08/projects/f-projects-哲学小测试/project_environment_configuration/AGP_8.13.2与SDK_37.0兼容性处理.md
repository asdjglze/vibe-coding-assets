---
title: "AGP 8.13.2与SDK 37.0兼容性处理"
usage_scenario:
    - "遇到AGP与新版SDK不兼容警告时"
    - "配置Gradle构建环境参数时"
keywords:
    - "AGP"
    - "SDK 37.0"
    - "兼容性警告"
    - "gradle.properties"
---

项目使用Android Gradle Plugin (AGP) 8.13.2编译compileSdk 37.0时会出现兼容性警告，需在gradle.properties中添加`android.suppressUnsupportedCompileSdk=37.0`以抑制该警告
