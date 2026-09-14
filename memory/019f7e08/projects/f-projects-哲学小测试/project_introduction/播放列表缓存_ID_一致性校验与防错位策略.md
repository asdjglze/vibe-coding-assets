---
title: "播放列表缓存 ID 一致性校验与防错位策略"
usage_scenario:
    - "排查播放列表显示混乱（文字/封面/音频不一致）问题"
    - "Airsonic 媒体库重建或 ID 变更后的数据同步检查"
    - "配置后端缓存策略以防止 ID 错位"
keywords:
    - "缓存校验"
    - "ID 重排"
    - "no-store"
    - "数据一致性"
---

播放列表缓存机制必须包含 ID 一致性校验：加载缓存时抽查首/中/尾歌曲，通过 getSong 实测 ID 当前指向内容，若标题不匹配则作废缓存并重新生成。同时 /api/stream 和 /api/cover 接口必须设置 Cache-Control: no-store 响应头，防止 Airsonic 数据库重建导致 ID 重排后前端仍播放旧缓存数据。
