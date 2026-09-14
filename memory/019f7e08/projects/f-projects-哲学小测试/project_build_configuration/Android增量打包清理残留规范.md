---
title: "Android增量打包清理残留规范"
usage_scenario:
    - "执行Android应用编译构建前"
    - "排查APK体积异常或发现包内存在废弃资源时"
keywords:
    - "AGP增量打包"
    - "构建缓存清理"
    - "APK残留"
---

Android项目使用Gradle构建时，增量打包机制（AGP）不会自动清理zip空洞或历史残留文件。为确保APK纯净，必须在执行 `gradlew assembleDebug` 前手动删除旧的 `app-debug.apk` 和 `app\build\intermediates\assets\debug` 目录，然后再进行完整构建。
