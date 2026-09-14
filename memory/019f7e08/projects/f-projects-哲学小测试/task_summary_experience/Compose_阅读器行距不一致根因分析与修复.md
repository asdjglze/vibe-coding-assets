---
title: "Compose 阅读器行距不一致根因分析与修复"
usage_scenario:
    - "排查 Compose 布局中行距异常或样式继承失效问题"
    - "修复多模式阅读器排版不一致的 Bug"
    - "需要统一不同渲染路径下字体行距的计算逻辑"
keywords:
    - "Compose"
    - "LocalTextStyle"
    - "行距修复"
    - "排版一致性"
---

## 任务描述
修复 Android Compose 阅读器中翻页模式与滚动模式行距不一致的问题。

## 执行过程
```mermaid
graph TD
    A[问题:翻页模式行距偏紧] --> B[对比滚动与翻页模式渲染链路]
    B --> C[发现 LocalTextStyle 机制差异]
    C --> D[定位根因:显式 style 参数挤掉 LocalTextStyle 行距]
    D --> E[分析 MaterialTheme 默认 Typography 配置]
    E --> F[确定修复策略:所有块显式设置 lineHeight=字号×倍率]
    F --> G[修改 PagedReaderBodies.kt 6 处 Para 渲染逻辑]
    G --> H[修改 MdBody.kt 和 ReaderScreen.kt 对齐滚动模式]
    H --> I[编译验证 KSP 构建成功]
```

## 任务总结
根因是 Compose Text 显式传入 style 参数会替换 LocalTextStyle，导致依赖该上下文的行距退化为自然行高。修复方案是在翻页端 (PagedReaderBodies.kt) 和滚动端 (MdBody.kt, ReaderScreen.kt) 的所有 Text 组件中，显式设置 lineHeight = fontSize * lineHeightRatio，确保两端行距公式完全一致且随设置动态变化。
