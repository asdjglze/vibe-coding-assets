---
title: "AGP 9.3.1与Compose 1.12.0兼容性处理"
usage_scenario:
    - "需要编译项目或运行构建命令时"
    - "排查AGP与Compose版本冲突导致的构建失败"
keywords:
    - "AGP 9.3.1"
    - "Compose 1.12.0"
    - "Gradle 9.5.0"
    - "版本冲突"
---

项目使用Android Gradle Plugin (AGP) 9.3.1编译时，依赖Compose 1.12.0稳定版同样因版本不兼容（要求AGP ≥ 9.1.0）导致构建失败；解决方案是将AGP升级至9.3.1+，配套Gradle升级至9.5.0
