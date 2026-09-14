---
title: "WebView预览与导出渲染层切换策略"
usage_scenario:
    - "Android WebView 预览卡顿且需要截取完整长图的场景"
    - "解决 Android WebView 导出图片黑屏或内容缺失的问题"
    - "需要在高性能预览和高保真截图之间权衡的 UI 组件开发"
keywords:
    - "WebView"
    - "渲染层切换"
    - "LAYER_TYPE_HARDWARE"
    - "LAYER_TYPE_SOFTWARE"
    - "截图黑屏"
---

## 任务描述
解决 Android WebView 分享卡片预览卡顿及导出图片失败（黑屏/空白）的问题。

## 执行过程
```mermaid
graph TD
    A[需求:解决预览卡顿与导出异常] --> B[分析原因:软件层光栅化导致预览慢,硬件层无法截取完整内容]
    B --> C[修改ShareCardWebView.kt:默认启用LAYER_TYPE_HARDWARE]
    C --> D[修改ShareCardScreen.kt:导出按钮点击时临时切换为LAYER_TYPE_SOFTWARE]
    D --> E[增加delay等待一帧光栅化完成]
    E --> F[调用captureShareCardWebView截取Bitmap]
    F --> G[截取完成后切回LAYER_TYPE_HARDWARE]
```

## 任务总结
实现了 WebView 渲染层的动态切换策略：
1. **预览阶段**：使用 `View.LAYER_TYPE_HARDWARE` (GPU)，利用硬件加速提升滚动和缩放性能，解决卡顿。
2. **导出阶段**：在点击导出按钮时，临时切换为 `View.LAYER_TYPE_SOFTWARE` (CPU)，确保 `view.draw()` 能捕获到完整的 WebView 内容（包括超出屏幕部分），避免黑屏或截断。截取完成后立即切回硬件层。
