---
title: "maoyulu项目打包脚本为build_pack.ps1"
usage_scenario:
    - "需要执行 maoyulu 项目的正式打包任务"
    - "排查构建脚本相关的问题"
keywords:
    - "打包脚本"
    - "build_pack.ps1"
    - "APK构建"
---

maoyulu 项目的**真正打包脚本是 `build_pack.ps1`**（位于项目根目录），而非 `build.bat`。`build.bat` 仅用于开发环境的单包构建。`build_pack.ps1` 负责构建三个档位的 APK（large/medium/mini），包含字体子集化、清理旧产物、assembleDebug 等完整流程。用法：`powershell -ExecutionPolicy Bypass -File build_pack.ps1`。（来源：Bash/Read）
