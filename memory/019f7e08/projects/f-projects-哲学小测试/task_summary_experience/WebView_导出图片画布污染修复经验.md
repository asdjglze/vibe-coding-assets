---
title: "WebView 导出图片画布污染修复经验"
usage_scenario:
    - "WebView 内嵌 HTML 导出 Canvas 图片时报 SecurityError"
    - "排查 Canvas.toDataURL 失败原因"
    - "解决 Blob URL 导致的画布跨源污染问题"
keywords:
    - "Canvas 污染"
    - "SecurityError"
    - "Blob URL"
    - "data URI"
    - "图片导出"
---

## 任务描述
修复分享卡片图片导出功能中的 SecurityError 错误（Tainted canvases may not be exported），解决超高卡片内容无法生成 PNG 的问题。

## 执行过程
```mermaid
graph TD
    A[需求:修复图片导出失败] --> B[浏览器复现并查看 Console 日志]
    B --> C[发现 toDataURL 抛 SecurityError: Tainted canvases]
    C --> D[排查污染源：CSS/HTML 中的 file:// 引用]
    D --> E[尝试将 file:// 资源转为 data: URI]
    E --> F[验证：转 data: 后仍报错，说明非 file:// 引用导致]
    F --> G[最小实验对比：Plain Canvas vs Blob SVG vs Data SVG]
    G --> H[定位根因：Blob URL 加载字体后触发 Chrome/WebView 跨源判定]
    H --> I[方案：改用 encodeURIComponent 直接拼接 data:image/svg+xml URI]
    I --> J[验证：6.7MB 超大 SVG 正常导出 36KB PNG]
```

## 任务总结
成功修复导出功能。根本原因是 Blob URL (`URL.createObjectURL`) 加载包含字体的 SVG 文档时，被浏览器判定为跨源导致画布污染。解决方案是将 SVG 字符串通过 `encodeURIComponent` 转换为 `data:image/svg+xml;charset=utf-8,` 前缀的 URI 赋值给 img.src，确保同源安全。同时保留了对 CSS/HTML 中相对 URL 的资源转 base64 处理逻辑。
