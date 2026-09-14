---
title: "AGP要求SDK Platform目录名需匹配ApiLevel"
usage_scenario:
    - "AGP构建时卡在下载或安装某个 Android Platform 包"
    - "本地已安装 SDK Platform 但 AGP 仍提示缺失或重复下载"
    - "解决 Android 项目编译缓慢且涉及 SDK 组件更新的问题"
keywords:
    - "AGP"
    - "SDK Platform"
    - "ApiLevel"
    - "目录命名"
---

AGP (Android Gradle Plugin) 要求 SDK Platform 目录名称必须与 source.properties 中定义的 ApiLevel 完全一致。例如，若 source.properties 中 `AndroidVersion.ApiLevel=37.0`，则目录名必须是 `android-37.0`。如果目录名是 `android-37`（不匹配），AGP 会认为平台包缺失并触发重新下载，导致构建极慢。解决方案：将已安装的完整平台包目录复制一份，命名为正确的格式（如从 `android-37` 复制为 `android-37.0`）。（来源：Bash, AGP构建日志）
