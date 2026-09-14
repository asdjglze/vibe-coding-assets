---
title: "多档位APK打包脚本配置"
usage_scenario:
    - "需要编译项目或运行构建命令时"
    - "运行测试前确定正确的构建命令"
keywords:
    - "build_pack.ps1"
    - "多档位APK"
    - "打包脚本"
---

项目使用 `build_pack.ps1` 脚本打包三个档位 APK（large/medium/mini），包含字体子集化、清理旧产物、assembleDebug 等完整流程。用法：`powershell -ExecutionPolicy Bypass -File build_pack.ps1`
