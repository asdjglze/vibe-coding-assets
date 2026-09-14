---
title: "Duolingo 徽章：火焰加年份数字规范"
usage_scenario:
    - "修改 Duolingo 模板徽章文案或逻辑时"
    - "实现或调整分享卡片年份显示功能时"
keywords:
    - "Duolingo"
    - "发布年份"
    - "徽章替换"
    - "动态隐藏"
---

Duolingo 分享模板的徽章：保留火焰图标（CSS ::before + SVG data URI 画白色火焰，App 内禁用 emoji），数字直接显示 year 字段值（无"年"后缀）；year 为空时整个徽章自动隐藏；橙底立体胶囊样式保留。火焰不可删除、年份数字不可加"年"字。
