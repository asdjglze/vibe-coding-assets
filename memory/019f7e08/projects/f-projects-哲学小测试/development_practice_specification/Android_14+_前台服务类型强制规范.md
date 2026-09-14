---
title: "Android 14+ 前台服务类型强制规范"
usage_scenario:
    - "修复 Android 14+ 设备应用启动闪退问题"
    - "编写或修改前台服务代码时确保兼容性"
    - "排查 targetSdk ≥ 34 应用的崩溃日志"
keywords:
    - "Android 14+"
    - "startForeground"
    - "前台服务类型"
    - "闪退"
---

Android 14+ (targetSdk ≥ 34) 调用 startForeground() 必须显式指定服务类型（如 FOREGROUND_SERVICE_TYPE_MEDIA_PLAYBACK），使用两参数版本会抛出 MissingForegroundServiceTypeException 导致应用启动即闪退
