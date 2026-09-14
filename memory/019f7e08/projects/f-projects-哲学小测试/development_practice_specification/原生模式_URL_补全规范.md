---
title: "原生模式 URL 补全规范"
usage_scenario:
    - "修复原生播放器播放失败问题"
    - "排查跨平台播放地址兼容性问题"
    - "实现原生与 Web 混合架构的媒体播放功能"
keywords:
    - "原生播放器"
    - "URL 补全"
    - "ExoPlayer"
    - "相对路径"
---

原生播放器接收相对路径 URL（如/api/stream/id）时，必须在调用播放前自动补全为绝对 URL（添加 scheme 和 host），否则 ExoPlayer 无法解析导致播放失败并触发自动跳过逻辑
