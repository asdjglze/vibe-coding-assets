---
title: "Compose 滚动性能优化：避免组合期状态读取"
usage_scenario:
    - "排查 Compose 列表滚动卡顿或掉帧问题时"
    - "优化包含动态状态读取的滚动子组件时"
    - "重构长文阅读页等高性能要求场景"
keywords:
    - "Compose"
    - "滚动掉帧"
    - "组合期重组"
    - "绘制阶段"
---

Compose 列表滚动掉帧主因是子组件在组合阶段读取滚动状态（如 scrollState.value），导致每帧触发重组、重测量和重布局；优化方案是将状态读取移至绘制阶段（drawBehind）或使用 derivedStateOf 降低重组频率，同时移除冗余嵌套布局（如 BoxWithConstraints）。
