---
title: "分享卡片 Web 侧转图与视口适配改造"
usage_scenario:
    - "处理 WebView 渲染内容过高导致原生导出 OOM 的场景"
    - "需要实现 WebView 内容自适应视口显示且不影响导出分辨率"
    - "排查分享卡片预览滚动异常或导出图片黑屏/模糊问题"
keywords:
    - "WebView 转图"
    - "OOM 修复"
    - "视口适配"
    - "分块传输"
---

## 任务描述
解决分享卡片超高内容时安卓侧导出图片 OOM 的问题，将导出方式从原生 View.draw 改为 Web 侧 Canvas 转图，同时优化预览层视口适配逻辑。

## 执行过程
```mermaid
graph TD
    A[需求:超高卡片导出 OOM + 预览适配] --> B[JS 层:添加 syncViewport 视口缩放逻辑]
    B --> C[JS 层:实现 exportCardImage 分块转图 (256KB chunks)]
    C --> D[Kotlin 层:ShareBridge 新增 onImageChunk 分块接收与重组]
    D --> E[Kotlin 层:移除 captureShareCardWebView 及软件层切换逻辑]
    E --> F[Kotlin 层:新增 exportImageFromWeb suspend 导出函数]
    F --> G[UI 层:ShareCardScreen 调整预览高度封顶为 min(自然高，视口高)]
    G --> H[验证:超高卡片导出正常且预览不滚动]
```

## 任务总结
成功重构分享卡片导出机制：JS 侧通过 CSS zoom 自适应视口，Canvas 转图后分块回传 Base64 避免大字符串阻塞；原生层移除 View.draw 截图，改用协程等待分块收齐生成 Bitmap。解决了超高卡片 OOM 问题，预览层自动缩放填满视口，无双重滚动卡顿。
