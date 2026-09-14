---
title: "WebView复用与页面内跳转优化"
usage_scenario:
    - "优化分享卡片模板切换速度时采用复用方案"
    - "排查WebView频繁重建导致的卡顿问题时参考此机制"
keywords:
    - "WebView复用"
    - "页面内跳转"
    - "性能优化"
---

分享卡片预览采用WebView复用机制：首次进入创建一次WebView实例，后续切换模板时直接在当前实例内重新加载HTML（loadDataWithBaseURL），不再销毁重建WebView组件，以消除WebView创建带来的性能开销。
