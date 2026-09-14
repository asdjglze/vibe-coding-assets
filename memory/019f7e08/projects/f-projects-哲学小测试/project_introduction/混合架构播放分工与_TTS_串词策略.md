---
title: "混合架构播放分工与 TTS 串词策略"
usage_scenario:
    - "设计或重构混合架构音乐播放功能时确定职责边界"
    - "处理锁屏场景下 TTS 串词与自动切歌的冲突逻辑"
    - "评估大规模重构风险并制定分阶段实施方案"
keywords:
    - "混合架构"
    - "播放队列"
    - "TTS 串词"
    - "分阶段重构"
---

混合架构音乐播放采用"原生管理队列、网页负责交互"的分工模式，分阶段实施：
- 阶段一已实施：原生影子队列（setPlaylist 同步 url/id/script+索引+模式）、模式感知兜底切歌（sequence/loop/shuffle/single）、兜底 TTS 原生化（JS 冻结时原生直接调 POST /api/playlist/tts 合成串词并播放，播完自动切歌，失败降级直接切歌）、getPlaybackState 同步桥供前端恢复校正、原生 30s 心跳兜底（WebView 后台 setInterval 节流时保持服务器在线）
- 前台 JS 存活时前端仍负责 TTS 任务编排与切歌（双轨无冲突，autoNextActive/autoNextTtsActive 标志保护）；TTS 串词握手协议：JS 冻结时原生接管
- 阶段二（控制权完全移交原生）未实施，待阶段一真机验证稳定后评估
- 前端 player.js 修改需 esbuild 重新构建部署服务器才生效（WebView 加载服务器页面）
