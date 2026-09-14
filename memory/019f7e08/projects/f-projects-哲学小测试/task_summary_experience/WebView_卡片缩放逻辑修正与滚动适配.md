---
title: "WebView 卡片缩放逻辑修正与滚动适配"
usage_scenario:
    - "Android WebView 展示长内容卡片时出现异常缩放或布局塌陷问题"
    - "需要同时支持多尺寸卡片预览且避免双重滚动冲突的场景"
    - "调整 WebView 视口适配策略（如从 contain 改为 width-only）"
keywords:
    - "WebView 缩放"
    - "高度封顶"
    - "外层滚动"
    - "卡片适配"
---

## 任务描述
修复 Android WebView 卡片在视口适配时因外部高度封顶导致内部 zoom 过度缩小（缩成点）的问题。

## 执行过程
```mermaid
graph TD
    A[用户反馈：卡片被缩成一个点] --> B[排查 Kotlin 侧是否有外部缩放代码]
    B --> C[确认 Kotlin 侧无缩放代码]
    C --> D[分析根因：外部 min(cardHeight, 视口高) 强制压扁 WebView]
    D --> E[推导后果：内部 zoom 被迫计算极小比例以填满受限高度]
    E --> F[制定方案：外部不做高度压缩，按自然高度显示]
    F --> G[修改 ShareCardScreen.kt: 移除 height(displayH) 限制]
    G --> H[添加外层 verticalScroll 支持超高卡片滚动查看]
    H --> I[保留 JS 侧逻辑：仅当宽度超出时进行等比微缩]
    I --> J[重新打包 APK 验证效果]
```

## 任务总结
成功解决卡片缩成点问题：移除 Kotlin 侧对 WebView 高度的 `minOf` 封顶限制，改为按卡片自然高度 (`cardHeight.dp`) 显示，并在外层容器添加 `verticalScroll` 以支持超长内容滚动。JS 侧保留仅针对宽度超出的缩放逻辑，彻底消除因外部尺寸压缩导致的内部过度缩放副作用。
