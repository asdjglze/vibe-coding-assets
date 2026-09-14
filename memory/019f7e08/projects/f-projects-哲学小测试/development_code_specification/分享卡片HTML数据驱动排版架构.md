---
title: "分享卡片HTML数据驱动排版架构"
usage_scenario:
    - "新增或修改分享卡片模板时遵循数据流约定"
    - "排查模板渲染问题时代入JSON数据结构"
keywords:
    - "JSON驱动"
    - "data-field"
    - "排版解耦"
    - "WebView"
---

分享卡片 HTML 模板采用 JSON 数据驱动排版机制：App 侧将内容打包为固定 JSON 对象注入 WebView，模板通过 data-field 属性声明字段占位，由公共 JS 脚本自动填充，App 不参与具体排版逻辑。
