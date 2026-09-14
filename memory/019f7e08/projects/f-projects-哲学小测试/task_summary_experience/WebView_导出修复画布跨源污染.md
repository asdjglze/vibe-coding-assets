---
title: "WebView 导出修复画布跨源污染"
usage_scenario:
    - "Android WebView 导出图片时出现 SecurityError/Tainted canvases 错误"
    - "需要处理本地 file:// 资源导致的 Canvas 跨域限制"
    - "修复 SVG 渲染后无法同步导出为 PNG 的问题"
keywords:
    - "Canvas 污染"
    - "file:// 资源"
    - "XHR 转 data URI"
---

## 任务描述
修复 Android WebView 中分享卡片图片导出时因引用本地 file:// 资源导致的 SecurityError (Tainted canvases)。

## 执行过程
```mermaid
graph TD
    A[需求:修复图片导出失败] --> B[复现问题:浏览器模拟 WebView 环境]
    B --> C[定位 Console 报错:Tainted canvases may not be exported]
    C --> D[分析根因:SVG 内 drawImage 引用 file:// 字体/图片导致跨源污染]
    D --> E[方案:将 file:// 资源转为 data: URI 实现同源]
    E --> F[实现 XHR 读取 blob 并转 base64]
    F --> G[替换 CSS/HTML 中的相对 URL 为 data: URI]
    G --> H[验证修复:Console 显示 assets 转换成功且无报错]
```

## 任务总结
成功解决导出失败问题：在 share_core.js 中增加 resource absolutize 和 convertOne 逻辑，通过 XHR 读取 file:// 资源转为 data: URI，消除 Canvas 跨源污染，确保 toDataURL 可正常执行。
