---
title: "构建前清理旧APK规范"
usage_scenario:
    - "编写或修改Android Gradle构建脚本时"
    - "排查重复APK残留问题时"
keywords:
    - "构建脚本"
    - "清理旧包"
    - "APK删除"
---

打包构建脚本（build.bat、build_pack.ps1）在执行 `assemble` 或 `build` 任务前，必须先执行清理命令删除旧的 APK 文件，然后再进行新的打包构建
