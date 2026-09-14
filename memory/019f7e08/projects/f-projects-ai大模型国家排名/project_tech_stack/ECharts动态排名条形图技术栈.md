---
title: "ECharts动态排名条形图技术栈"
usage_scenario:
    - "了解项目所依赖的核心可视化库及其关键能力与限制"
keywords:
    - "ECharts"
    - "rich文本"
    - "realtimeSort"
    - "category轴"
    - "setOption"
---

前端技术栈：
- 图表库：ECharts（核心依赖）
- 关键特性使用：`rich`富文本渲染国旗、`realtimeSort`排序动画、`setOption`动态更新、category类型Y轴
- 技术约束：category Y轴不支持位置交换动画，导致排名动画需特殊处理
