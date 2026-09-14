---
title: "构建脚本SDK目录名适配"
usage_scenario:
    - "排查构建脚本因SDK目录名不匹配导致的误报错"
    - "修改或维护构建脚本时确认正确的SDK路径"
keywords:
    - "build.bat"
    - "SDK检查"
    - "android-37.0"
---

项目构建脚本build.bat中的Android SDK Platform检查逻辑需适配`android-37.0`目录名（而非旧版`android-37`），否则会导致误报缺失并中断构建
