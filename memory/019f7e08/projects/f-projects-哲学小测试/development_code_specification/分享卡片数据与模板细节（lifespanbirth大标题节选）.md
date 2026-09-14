---
title: "分享卡片数据与模板细节（lifespan/birth/大标题/节选）"
usage_scenario:
    - "调整55东方留白模板布局时应用书名字号与印章样式"
    - "修复印章形状不符合设计稿的问题"
keywords:
    - "东方留白"
    - "朱印椭圆"
    - "毛体思"
    - "书名放大"
---

分享卡片数据与模板细节：
- lifespan 字段为纯年份「1881—1936」（生卒年，编者留空），数据源在 Theme.kt StylePresets；出生年月是独立字段 birth（如「1881.09」「1893.12」，无则空），仅供需出生月的模板（如 67 喜马拉雅 FM 频段号）直接读取，不要混入 lifespan 字符串，避免其他模板加提取代码；正则 \d{3,4} 提取年份逻辑兼容两种格式。
- 66 网易云：ne-vinyl 圆形内 ♪（::after content \266A 16px 红）；ne-logo .icon 毛体思（MaoXing 字体 + 红圈，HTML 放「思」字）；ne-quote padding 32px 上 48px 下避让左上 icon 与底部 meta。
- 67 喜马拉雅：xm-quote padding 30px 上 70px 下（大留白）；xm-sub 已删；label 的 FM 频段号由模板内联脚本从 birth 字段提取出生年月去首字符（1881.09→881.9），无数据时保留默认 102.4。
- 大标题两行控制：share_core.js 提供 data-fit-title（在 data-field 元素上生效）+ data-fit-lines/data-fit-min；行数判定 getClientRects（inline 数行，flex 块化按高度/行高折算），超过 maxLines 逐次 ×0.9 缩字号到 min 下限。75/79/74/80 标题已启用。
- 节选文案统一英文「Excerpt shared in 当前年」：data-field="year" data-now-year="1" data-prefix="Excerpt shared in "（data-now-year 直接填分享当年份，不用作品年）。
