---
title: "Compose 长列表滚动性能优化规范"
usage_scenario:
    - "开发包含长文本或大量数据的滚动界面时排查掉帧问题"
    - "优化 UI 组件以减少不必要的重组和重绘开销"
keywords:
    - "Compose"
    - "滚动优化"
    - "重组控制"
    - "绘制阶段"
---

在 Compose 长列表滚动场景中，避免在组合阶段读取状态值导致每帧重组；应将状态读取移至绘制阶段（drawBehind）或使用 derivedStateOf 减少重组频率，同时移除冗余嵌套布局以消除掉帧。
