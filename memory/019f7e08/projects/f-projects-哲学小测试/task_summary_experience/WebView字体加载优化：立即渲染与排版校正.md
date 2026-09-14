---
title: "WebView字体加载优化：立即渲染与排版校正"
usage_scenario:
    - "Android WebView中HTML模板字体加载导致的白屏或逐行加载问题"
    - "前端页面字体加载时序优化，避免FOIT/FOUT造成的视觉闪烁"
    - "跨平台H5模板引擎中处理自定义字体的通用方案"
keywords:
    - "字体加载"
    - "font-display"
    - "排版校正"
    - "WebView优化"
---

## 任务描述
解决移动端 WebView 中因等待 @font-face 字体加载导致的页面逐行加载（类似图片缓慢加载）问题，改为立即用回退字体渲染，字体就绪后再进行排版校正。

## 执行过程
```mermaid
graph TD
    A[需求:消除字体加载导致的逐行加载感] --> B[分析根因:JS等待document.fonts.ready才填充]
    B --> C[修改share_core.js:改为立即fillAll]
    C --> D[新增refit函数:重置内联样式并按新字体宽度重算]
    D --> E[修改ShareTemplateRepository.kt:注入font-display:swap]
    E --> F[编译验证构建]
```

## 任务总结
成功优化字体渲染策略：
1. **JS层**：移除对 `document.fonts.ready` 的阻塞等待，改为立即渲染；字体就绪后调用 `refit()` 重置内联样式（fontSize, letterSpacing等）并重新适配布局。
2. **Kotlin层**：在 `loadHtml` 方法中通过正则将 `@font-face` 替换为 `@font-face{font-display:swap;`，确保浏览器优先显示回退字体。
3. 效果：页面一步到位显示文字，无空白等待，无整卡重排闪烁。
