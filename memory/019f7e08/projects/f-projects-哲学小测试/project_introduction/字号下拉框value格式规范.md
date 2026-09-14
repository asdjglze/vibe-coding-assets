---
title: "字号下拉框value格式规范"
usage_scenario:
    - "配置前端字号选择器HTML时确保option value格式统一"
    - "修复下拉框默认值不显示问题时定位根源"
    - "前端UI组件开发中处理数字型value与JS赋值一致性"
keywords:
    - "下拉框"
    - "value格式"
    - "3.7 vs 3.70"
    - "字号选择"
---

前端字号下拉框（如'正文字号'设置）的option value必须使用无尾零数字格式（如'3.7'而非'3.70'），否则JS赋值select.value=3.7时无法匹配value='3.70'，导致下拉框显示为空。
