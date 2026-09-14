---
title: "UI组件复用现有库规范"
usage_scenario:
    - "重构或新增翻页、滑动等交互功能时"
    - "评估是否引入第三方库还是自研实现时"
keywords:
    - "UI实现"
    - "库复用"
    - "禁止自绘"
---

开发中涉及翻页、列表滚动等UI交互时，必须优先使用现有的成熟库或官方组件（如Jetpack Compose的VerticalPager）进行实现，严禁自行编写底层渲染逻辑
