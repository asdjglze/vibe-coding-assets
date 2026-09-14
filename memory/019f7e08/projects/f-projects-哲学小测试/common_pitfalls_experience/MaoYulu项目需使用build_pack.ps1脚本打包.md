---
title: "MaoYulu项目需使用build_pack.ps1脚本打包"
usage_scenario:
    - "需要打包 MaoYulu Android 应用时"
    - "发现生成的 APK 体积异常大（超过 80MB）时"
keywords:
    - "MaoYulu"
    - "打包脚本"
    - "APK体积"
    - "build_pack.ps1"
---

MaoYulu 项目打包必须使用项目根目录的 `build_pack.ps1` 脚本，禁止直接使用 `gradlew assembleDebug`。直接 gradle 构建会包含全量字体和未优化的资源，导致 APK 体积异常大（>90MB）。现有脚本会自动执行字体子集化、语料库同步及三档（mini/medium/large）打包，产物位于 `dist/` 目录。（来源：Bash, Read）
