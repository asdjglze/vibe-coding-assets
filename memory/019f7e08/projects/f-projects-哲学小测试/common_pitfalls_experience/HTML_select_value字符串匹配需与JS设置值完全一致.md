---
title: "HTML select value字符串匹配需与JS设置值完全一致"
usage_scenario:
    - "下拉框默认值不显示或设置后空白"
    - "JS动态设置select.value无效但无报错"
    - "数值型选项value含尾随零（如3.70）导致匹配失败"
keywords:
    - "select value"
    - "字符串匹配"
    - "数值精度"
    - "HTML下拉框"
---

前端下拉框（<select>）的 value 属性必须与 JS 中设置的 select.value 字符串完全一致才能正确选中。JS 数字 3.70 与 3.7 相等，但 HTML 中 value="3.70" 作为字符串与 JS 设置的 "3.7" 不匹配，导致选项无法高亮显示。解决方案：将所有相关 HTML option 的 value 和 JS 数据源（如 config.js 中的 mm 字段）统一改为无尾随零的数值表示，例如 "3.70" → "3.7"。（来源：SearchReplace）
