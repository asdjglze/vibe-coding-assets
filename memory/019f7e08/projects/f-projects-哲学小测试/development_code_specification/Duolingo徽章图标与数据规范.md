---
title: "Duolingo徽章图标与数据规范"
usage_scenario:
    - "开发或维护Duolingo分享模板时确保徽章元素正确"
    - "处理元数据缺失时的组件降级逻辑"
keywords:
    - "Duolingo模板"
    - "SVG图标"
    - "年份显示"
    - "徽章隐藏"
---

Duolingo徽章实现规范：火焰图标必须使用SVG伪元素（白色火焰）替代emoji；徽章内的数字直接显示年份值（如1925），严禁添加"年"后缀；数据降级链：year 字段有值显示 year → 无 year 则回退显示作者出生年（share_core.js data-fallback-birth="1"，从 lifespan 提取首组数字）→ lifespan 也无则整个徽章隐藏。
