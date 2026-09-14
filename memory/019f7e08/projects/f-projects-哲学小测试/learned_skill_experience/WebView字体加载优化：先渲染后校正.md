---
title: "WebView字体加载优化：先渲染后校正"
usage_scenario:
    - "WebView页面因等待字体加载导致白屏或卡顿"
    - "自定义字体加载慢影响首屏渲染速度"
    - "需要消除字体切换时的布局抖动"
keywords:
    - "font-display"
    - "swap"
    - "字体加载"
    - "WebView优化"
    - "排版校正"
---

## 输入
- 需要优化的HTML页面（特别是包含自定义字体的页面）

## 步骤
1. **移除字体等待机制**：删除 `document.fonts.ready` 的监听逻辑，不再等待字体加载完成再渲染。
2. **立即渲染**：在脚本初始化阶段直接调用 `run()` 或等效函数，使用系统回退字体一步到位渲染文字。
3. **注入 font-display:swap**：在 HTML 模板中通过正则替换，将所有 `@font-face {` 修改为 `@font-face{font-display:swap;`，确保字体加载期间文字可见。
4. **排版校正**：在 `document.fonts.ready` 回调中仅执行一次排版校正（如重算字号、换行、清除内联样式），不重复填充内容。

## 输出
页面文字立即显示，无白屏或逐行刷出效果；字体加载完成后无缝替换，布局自动微调。

## 注意事项
- **禁止整卡重填**：字体就绪后只做样式修正，避免触发 DOM 重绘导致卡顿。
- **结构操作不可重入**：确保填充逻辑只执行一次，防止重复插入元素。
- **来源工具**：SearchReplace (share_core.js, ShareTemplateRepository.kt)
