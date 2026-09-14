---
title: "CSS white-space解决多行文本折叠问题"
usage_scenario:
    - "HTML文本含换行符但渲染时变成一坨"
    - "多行文本在浏览器中未正确换行显示"
keywords:
    - "CSS"
    - "white-space"
    - "换行"
    - "渲染"
---

HTML中若文本包含换行符（如\n）但浏览器将其折叠成空格显示为一坨，是因为元素缺少 `white-space: pre-line` 或 `pre-wrap`。默认情况下，浏览器会折叠连续空白符并将换行视为空格。修复方法：在对应CSS类中添加 `white-space: pre-line;`（保留换行和全角空格，折叠其他多余空格）或 `white-space: pre-wrap;`（保留所有空白）。来源：Bash/前端调试
