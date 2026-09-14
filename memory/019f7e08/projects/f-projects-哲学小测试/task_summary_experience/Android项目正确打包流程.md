---
title: "Android项目正确打包流程"
usage_scenario:
    - "需要生成测试或发布APK时"
    - "遇到APK体积异常大（>80MB）需排查构建方式时"
keywords:
    - "打包"
    - "build_pack.ps1"
    - "APK体积"
    - "构建规范"
---

Android项目打包严禁直接使用 `gradlew assembleDebug`，这会包含全量资源导致APK体积过大（通常>90MB）。必须使用项目根目录下的 `build_pack.ps1` 脚本。该脚本负责：
1. 同步语料库数据库（quote.db）。
2. 运行字体子集化脚本（subset_fonts.py）减小字体体积。
3. 生成三个档位（large/medium/mini）的APK，默认推荐安装 mini 档（约70MB）。
