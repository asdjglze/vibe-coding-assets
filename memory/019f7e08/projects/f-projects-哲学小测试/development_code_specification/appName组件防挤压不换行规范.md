---
title: "appName组件防挤压不换行规范"
usage_scenario:
    - "新增品牌类分享模板时配置appName样式"
    - "排查软件名称被截断或换行的布局问题"
keywords:
    - "appName"
    - "不换行"
    - "flex-shrink"
    - "布局挤压"
---

分享卡片中所有appName（软件名）组件必须保证完全显示不换行：添加 `flex-shrink: 0` 和 `white-space: nowrap`；左侧内容区域需设置 `min-width: 0` 允许收缩，防止挤压appName导致换行或截断。
