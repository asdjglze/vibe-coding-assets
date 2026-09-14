---
title: "GitHub 模板间距与代码标签行内样式规范"
usage_scenario:
    - "修改品牌类模板布局时调整间距参数"
    - "处理文本容器换行或溢出问题时应用块级化方案"
keywords:
    - "GitHub 模板"
    - "间距调整"
    - "block 布局"
    - "文本溢出"
---

GitHub 模板间距与正文渲染规范（以原 demo 395bb8 为准）：
- 头部 gap 为 6px（原 demo 值），不可擅自缩减
- 正文 span 必须转为 block 显示（防 inline 宽度塌陷），但 code 标签必须保持原 demo 的行内样式（padding 0 4px、无 display:block、无 margin-top），不可改为块级代码块
- span 与 code 必须写在 HTML 同一行：body 有 white-space:pre-wrap，换行空白文本节点会被渲染成空行，在正文与代码行之间产生 30px 空档
