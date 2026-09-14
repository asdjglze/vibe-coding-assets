---
title: "GitHub 模板 code 标签禁用块级且源码需同行为一行"
usage_scenario:
    - "修改 GitHub 分享模板时发现正文与代码间出现异常大空档"
    - "调整代码引用块样式后布局间距异常扩大"
keywords:
    - "code 标签"
    - "块级样式"
    - "white-space"
    - "间距修复"
---

GitHub 分享模板中 `code` 标签必须保持**行内样式**（`display: inline` 或默认），禁止改为块级（`display: block; margin-top: 8px`），否则正文与代码之间会出现巨大空档（实测 57px）。正确写法：`background: #EFF1F3; padding: 0 4px; border-radius: 3px; font-size: 12px;`。同时，HTML 源码中 `<span>` 与 `<code>` 标签必须写在同一行，避免 `white-space: pre-wrap` 将换行空白渲染成空行（实测 30px 空档）。（来源：GitHub 模板对比修复）
