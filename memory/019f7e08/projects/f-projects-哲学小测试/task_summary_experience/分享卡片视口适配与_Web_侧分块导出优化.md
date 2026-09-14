---
title: "分享卡片视口适配与 Web 侧分块导出优化"
usage_scenario:
    - "Android WebView 内嵌长内容卡片预览高度溢出处理"
    - "大尺寸内容导出图片时防止 OOM 的分块传输方案"
    - "Web 侧 Canvas 转图替代原生 view.draw 截图的实现"
keywords:
    - "WebView 视口适配"
    - "分块导出"
    - "OOM 修复"
    - "Canvas 转图"
---

## 任务描述
修复分享卡片在 WebView 中预览时高度溢出视图的问题，并解决几百行内容导致安卓侧导出图片 OOM 的崩溃问题。

## 执行过程
```mermaid
graph TD
    A[需求:修复预览溢出与导出 OOM] --> B[定位 ShareCardScreen/WebView/JS 模板代码]
    B --> C[JS 侧:新增 syncViewport 函数实现 CSS zoom 视口适配]
    C --> D[JS 侧:移除 ResizeObserver 避免循环上报]
    D --> E[JS 侧:重构 exportCardImage 为 canvas+SVG foreignObject 转图]
    E --> F[JS 侧:实现 base64 分块回传机制 (256KB/块) 防止桥卡死]
    F --> G[Kotlin 侧:ShareBridge 新增 onImageChunk 分块收集逻辑]
    G --> H[Kotlin 侧:WebView 从软件层改为硬件层渲染]
    H --> I[Kotlin 侧:移除 view.draw 截图逻辑，改用 JS 回调合成图片]
```

## 任务总结
成功根治预览溢出与导出崩溃：1. JS 侧通过 CSS zoom 将超高卡片缩小填满视口，WebView 自身不扩高；2. 导出改为 Web 侧 Canvas 转 SVG 再转 PNG，并通过分块传输 Base64 避免内存溢出；3. Android 侧 WebView 启用硬件加速，移除低效的 view.draw 截图方式。
